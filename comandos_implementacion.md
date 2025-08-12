# 🚀 Comandos de Implementación - Datalake AWS

## 📋 **Setup Inicial AWS**

### **1. Configuración de AWS CLI**
```bash
# Instalar AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Configurar credenciales
aws configure
# AWS Access Key ID: [TU_ACCESS_KEY]
# AWS Secret Access Key: [TU_SECRET_KEY]
# Default region name: eu-west-1
# Default output format: json
```

### **2. Crear VPC y Subnets**
```bash
# Crear VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=datalake-vpc}]'

# Crear subnets públicas
aws ec2 create-subnet --vpc-id vpc-xxxxx --cidr-block 10.0.1.0/24 --availability-zone eu-west-1a
aws ec2 create-subnet --vpc-id vpc-xxxxx --cidr-block 10.0.2.0/24 --availability-zone eu-west-1b

# Crear subnets privadas
aws ec2 create-subnet --vpc-id vpc-xxxxx --cidr-block 10.0.3.0/24 --availability-zone eu-west-1a
aws ec2 create-subnet --vpc-id vpc-xxxxx --cidr-block 10.0.4.0/24 --availability-zone eu-west-1b
```

### **3. Crear S3 Buckets**
```bash
# Crear buckets del datalake
aws s3 mb s3://empresa-datalake-landing-prod --region eu-west-1
aws s3 mb s3://empresa-datalake-raw-prod --region eu-west-1
aws s3 mb s3://empresa-datalake-master-prod --region eu-west-1
aws s3 mb s3://empresa-datalake-airflow-prod --region eu-west-1
aws s3 mb s3://empresa-datalake-athena-prod --region eu-west-1

# Configurar versioning
aws s3api put-bucket-versioning --bucket empresa-datalake-landing-prod --versioning-configuration Status=Enabled
aws s3api put-bucket-versioning --bucket empresa-datalake-raw-prod --versioning-configuration Status=Enabled
aws s3api put-bucket-versioning --bucket empresa-datalake-master-prod --versioning-configuration Status=Enabled
```

## 🐳 **Setup de Contenedores**

### **4. Crear ECR Repositories**
```bash
# Crear repositorios ECR
aws ecr create-repository --repository-name python-etl --region eu-west-1
aws ecr create-repository --repository-name spark-etl --region eu-west-1

# Obtener login token
aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin [ACCOUNT_ID].dkr.ecr.eu-west-1.amazonaws.com
```

### **5. Build y Push de Imágenes**
```bash
# Build Python container
cd PythonScriptsData/
docker build -t python-etl .
docker tag python-etl:latest [ACCOUNT_ID].dkr.ecr.eu-west-1.amazonaws.com/python-etl:latest
docker push [ACCOUNT_ID].dkr.ecr.eu-west-1.amazonaws.com/python-etl:latest

# Build Spark container
cd ../SparkScriptsData/
docker build -t spark-etl .
docker tag spark-etl:latest [ACCOUNT_ID].dkr.ecr.eu-west-1.amazonaws.com/spark-etl:latest
docker push [ACCOUNT_ID].dkr.ecr.eu-west-1.amazonaws.com/spark-etl:latest
```

## 🔄 **Setup de Airflow**

### **6. Crear EC2 Instance**
```bash
# Crear key pair
aws ec2 create-key-pair --key-name datalake-key --query 'KeyMaterial' --output text > datalake-key.pem
chmod 400 datalake-key.pem

# Crear security group
aws ec2 create-security-group --group-name datalake-sg --description "Security group for datalake" --vpc-id vpc-xxxxx

# Agregar reglas de seguridad
aws ec2 authorize-security-group-ingress --group-id sg-xxxxx --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id sg-xxxxx --protocol tcp --port 8080 --cidr 0.0.0.0/0

# Lanzar instancia EC2
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \
  --count 1 \
  --instance-type t2.large \
  --key-name datalake-key \
  --security-group-ids sg-xxxxx \
  --subnet-id subnet-xxxxx \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=datalake-airflow}]'
```

### **7. Instalar Docker en EC2**
```bash
# Conectar a la instancia
ssh -i datalake-key.pem ec2-user@[IP_INSTANCE]

# Instalar Docker
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user

# Instalar Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### **8. Configurar Airflow**
```bash
# Crear directorios
mkdir -p ~/airflow/{dags,logs,plugins,config}

# Copiar docker-compose.yaml
# (Usar el archivo del repositorio)

# Configurar variables de entorno
export AIRFLOW_UID=50000
export AIRFLOW_PROJ_DIR=~/airflow

# Inicializar Airflow
docker-compose up airflow-init

# Levantar servicios
docker-compose up -d
```

## 🔧 **Configuración de ECS**

### **9. Crear ECS Clusters**
```bash
# Crear cluster Python
aws ecs create-cluster --cluster-name python-etl-cluster --region eu-west-1

# Crear cluster Spark
aws ecs create-cluster --cluster-name spark-etl-cluster --region eu-west-1

# Crear task definitions
aws ecs register-task-definition --cli-input-json file://python-task-definition.json
aws ecs register-task-definition --cli-input-json file://spark-task-definition.json
```

### **10. Configurar ECS Services**
```bash
# Crear servicios ECS
aws ecs create-service \
  --cluster python-etl-cluster \
  --service-name python-etl-service \
  --task-definition python-etl:1 \
  --desired-count 0 \
  --region eu-west-1

aws ecs create-service \
  --cluster spark-etl-cluster \
  --service-name spark-etl-service \
  --task-definition spark-etl:1 \
  --desired-count 0 \
  --region eu-west-1
```

## 📚 **Setup de AWS Glue**

### **11. Crear Glue Database**
```bash
# Crear base de datos
aws glue create-database --database-input Name=datalake-db --region eu-west-1

# Crear crawlers
aws glue create-crawler \
  --name landing-crawler \
  --database-name datalake-db \
  --targets S3Targets=[{Path=s3://empresa-datalake-landing-prod}] \
  --role arn:aws:iam::[ACCOUNT_ID]:role/AWSGlueServiceRole \
  --region eu-west-1

aws glue create-crawler \
  --name raw-crawler \
  --database-name datalake-db \
  --targets S3Targets=[{Path=s3://empresa-datalake-raw-prod}] \
  --role arn:aws:iam::[ACCOUNT_ID]:role/AWSGlueServiceRole \
  --region eu-west-1

aws glue create-crawler \
  --name master-crawler \
  --database-name datalake-db \
  --targets S3Targets=[{Path=s3://empresa-datalake-master-prod}] \
  --role arn:aws:iam::[ACCOUNT_ID]:role/AWSGlueServiceRole \
  --region eu-west-1
```

## 🔍 **Setup de Athena**

### **12. Configurar Athena Workgroup**
```bash
# Crear workgroup
aws athena create-work-group \
  --work-group-name datalake-workgroup \
  --work-group-configuration ResultConfiguration={OutputLocation=s3://empresa-datalake-athena-prod/} \
  --region eu-west-1

# Configurar como default
aws athena update-work-group \
  --work-group datalake-workgroup \
  --work-group-configuration-updates EnforceWorkGroupConfiguration=true \
  --region eu-west-1
```

## 📈 **Setup de QuickSight**

### **13. Configurar QuickSight**
```bash
# Habilitar QuickSight (via consola web)
# Crear cuenta de administrador
# Configurar permisos de S3 y Athena
# Crear datasets desde Athena
# Crear dashboards iniciales
```

## 🚀 **Despliegue de DAGs**

### **14. Copiar DAGs a Airflow**
```bash
# Copiar DAGs desde repositorio
scp -i datalake-key.pem -r DagsAirflowData/dags/* ec2-user@[IP_INSTANCE]:~/airflow/dags/

# Copiar configuraciones
scp -i datalake-key.pem -r DagsAirflowData/config/* ec2-user@[IP_INSTANCE]:~/airflow/config/

# Reiniciar Airflow
ssh -i datalake-key.pem ec2-user@[IP_INSTANCE] "cd ~/airflow && docker-compose restart"
```

## 📊 **Testing y Validación**

### **15. Probar Pipeline ETL**
```bash
# Ejecutar DAG de prueba
curl -X POST http://[IP_INSTANCE]:8080/api/v1/dags/email_etl_pipeline/dagRuns \
  -H "Content-Type: application/json" \
  -d '{"conf": {"test_mode": true}}'

# Verificar logs
ssh -i datalake-key.pem ec2-user@[IP_INSTANCE] "cd ~/airflow && docker-compose logs -f airflow-scheduler"
```

### **16. Verificar Datos en S3**
```bash
# Listar archivos en buckets
aws s3 ls s3://empresa-datalake-landing-prod/ --recursive
aws s3 ls s3://empresa-datalake-raw-prod/ --recursive
aws s3 ls s3://empresa-datalake-master-prod/ --recursive

# Verificar metadatos en Glue
aws glue get-tables --database-name datalake-db --region eu-west-1
```

## 🔒 **Configuración de Seguridad**

### **17. Configurar IAM Roles**
```bash
# Crear rol para ECS tasks
aws iam create-role --role-name ecs-task-execution-role --assume-role-policy-document file://trust-policy.json

# Adjuntar políticas
aws iam attach-role-policy --role-name ecs-task-execution-role --policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy

# Crear política personalizada para S3
aws iam create-policy --policy-name datalake-s3-policy --policy-document file://s3-policy.json

# Adjuntar política al rol
aws iam attach-role-policy --role-name ecs-task-execution-role --policy-arn arn:aws:iam::[ACCOUNT_ID]:policy/datalake-s3-policy
```

## 📊 **Monitoreo y Alertas**

### **18. Configurar CloudWatch**
```bash
# Crear dashboard
aws cloudwatch put-dashboard \
  --dashboard-name datalake-monitoring \
  --dashboard-body file://dashboard-config.json \
  --region eu-west-1

# Crear alarmas
aws cloudwatch put-metric-alarm \
  --alarm-name datalake-ec2-cpu \
  --alarm-description "Alarm when EC2 CPU exceeds 80%" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --region eu-west-1
```

## 🔄 **CI/CD Pipeline**

### **19. Configurar GitHub Actions**
```yaml
# .github/workflows/deploy.yml
name: Deploy to AWS
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: eu-west-1
    - name: Build and push Docker images
      run: |
        docker build -t python-etl ./PythonScriptsData/
        docker tag python-etl:latest ${{ secrets.ECR_REGISTRY }}/python-etl:latest
        docker push ${{ secrets.ECR_REGISTRY }}/python-etl:latest
    - name: Deploy to ECS
      run: |
        aws ecs update-service --cluster python-etl-cluster --service python-etl-service --force-new-deployment
```

## 📋 **Checklist de Implementación**

- [ ] Setup inicial AWS (cuenta, SSO, IAM)
- [ ] Crear VPC y subnets
- [ ] Crear buckets S3
- [ ] Crear repositorios ECR
- [ ] Build y push de imágenes Docker
- [ ] Crear instancia EC2
- [ ] Instalar Docker y Docker Compose
- [ ] Configurar Airflow
- [ ] Crear clusters ECS
- [ ] Configurar task definitions
- [ ] Crear servicios ECS
- [ ] Configurar AWS Glue
- [ ] Configurar Athena
- [ ] Configurar QuickSight
- [ ] Desplegar DAGs
- [ ] Configurar monitoreo
- [ ] Testing y validación
- [ ] Configurar CI/CD
- [ ] Documentación y capacitación

---

**⚠️ Nota**: Reemplazar `[ACCOUNT_ID]`, `[IP_INSTANCE]`, `vpc-xxxxx`, `subnet-xxxxx`, `sg-xxxxx` con los valores reales de tu cuenta AWS. 