# 💰 Resumen Detallado de Costos - Datalake AWS

## 📊 **Desglose de Costos por Categoría**

### **1. SETUP INICIAL AWS** 
- **Mano de Obra**: 1,320.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 1,320.00 EUR

### **2. INFRAESTRUCTURA COMPUTE**
- **Mano de Obra**: 0.00 EUR
- **AWS Mensual**: 54.79 EUR
- **Total**: 54.79 EUR

### **3. ALMACENAMIENTO S3**
- **Mano de Obra**: 0.00 EUR
- **AWS Mensual**: 23.40 EUR
- **Total**: 23.40 EUR

### **4. CONTENEDORES**
- **Mano de Obra**: 0.00 EUR
- **AWS Mensual**: 18.92 EUR
- **Total**: 18.92 EUR

### **5. ORQUESTACIÓN**
- **Mano de Obra**: 3,360.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 3,360.00 EUR

### **6. CATALOGADO Y CONSULTAS**
- **Mano de Obra**: 0.00 EUR
- **AWS Mensual**: 301.38 EUR
- **Total**: 301.38 EUR

### **7. VISUALIZACIÓN**
- **Mano de Obra**: 2,880.00 EUR
- **AWS Mensual**: 46.25 EUR
- **Total**: 2,926.25 EUR

### **8. REDES Y SEGURIDAD**
- **Mano de Obra**: 0.00 EUR
- **AWS Mensual**: 45.00 EUR
- **Total**: 45.00 EUR

### **9. MONITOREO Y LOGS**
- **Mano de Obra**: 0.00 EUR
- **AWS Mensual**: 5.80 EUR
- **Total**: 5.80 EUR

### **10. BACKUP Y DISASTER RECOVERY**
- **Mano de Obra**: 480.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 480.00 EUR

### **11. DESARROLLO ETL**
- **Mano de Obra**: 7,440.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 7,440.00 EUR

### **12. INTEGRACIÓN**
- **Mano de Obra**: 1,680.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 1,680.00 EUR

### **13. TESTING Y VALIDACIÓN**
- **Mano de Obra**: 2,160.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 2,160.00 EUR

### **14. DEPLOYMENT**
- **Mano de Obra**: 2,160.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 2,160.00 EUR

### **15. CAPACITACIÓN**
- **Mano de Obra**: 1,680.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 1,680.00 EUR

### **16. SOPORTE Y MANTENIMIENTO**
- **Mano de Obra**: 7,200.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 7,200.00 EUR

### **17. PROYECTO**
- **Mano de Obra**: 10,960.00 EUR
- **AWS Mensual**: 0.00 EUR
- **Total**: 10,960.00 EUR

---

## 💵 **RESUMEN FINANCIERO**

### **📈 COSTOS DE IMPLEMENTACIÓN (UNA VEZ)**
| Concepto | Monto (EUR) |
|----------|-------------|
| **Setup Inicial AWS** | 1,320.00 |
| **Orquestación** | 3,360.00 |
| **Backup y Disaster Recovery** | 480.00 |
| **Desarrollo ETL** | 7,440.00 |
| **Integración** | 1,680.00 |
| **Testing y Validación** | 2,160.00 |
| **Deployment** | 2,160.00 |
| **Capacitación** | 1,680.00 |
| **Soporte y Mantenimiento** | 7,200.00 |
| **Proyecto** | 10,960.00 |
| **🔴 TOTAL IMPLEMENTACIÓN** | **38,440.00** |

### **🔄 COSTOS MENSUALES AWS (RECURRENTES)**
| Concepto | Monto (EUR) |
|----------|-------------|
| **Infraestructura Compute** | 54.79 |
| **Almacenamiento S3** | 23.40 |
| **Contenedores** | 18.92 |
| **Catalogado y Consultas** | 301.38 |
| **Visualización** | 46.25 |
| **Redes y Seguridad** | 45.00 |
| **Monitoreo y Logs** | 5.80 |
| **🔴 TOTAL MENSUAL AWS** | **495.14** |

### **📊 COSTOS ANUALES**
| Concepto | Monto (EUR) |
|----------|-------------|
| **Implementación (una vez)** | 38,440.00 |
| **AWS Anual (12 meses)** | 5,941.68 |
| **🔴 TOTAL PRIMER AÑO** | **44,381.68** |
| **🔴 TOTAL AÑOS SUBSIGUIENTES** | **5,941.68** |

---

## 🎯 **ANÁLISIS DE COSTOS**

### **💡 Puntos Clave:**
- **Implementación**: 38,440 EUR (una vez)
- **Operación Mensual**: 495 EUR
- **Operación Anual**: 5,942 EUR
- **Mayor Costo AWS**: Athena Data Scanned (250 EUR/mes) - representa el 50% del costo mensual
- **Mayor Costo Mano de Obra**: Desarrollo ETL (7,440 EUR)

### **📉 Optimizaciones Posibles:**
1. **Reducir consultas Athena**: Implementar caching y optimización de queries
2. **Comprimir datos S3**: Usar formatos más eficientes (Parquet + Snappy)
3. **Lifecycle policies**: Mover datos antiguos a S3-IA o Glacier
4. **Reserved Instances**: Para EC2 si se mantiene más de 1 año

### **📈 Escalabilidad:**
- **Costos fijos**: ~130 EUR/mes (EC2, NAT Gateway, QuickSight)
- **Costos variables**: ~365 EUR/mes (Athena, S3, ECS, CloudWatch)
- **Escalado lineal**: Los costos crecen proporcionalmente al volumen de datos

---

## 🔍 **DETALLE DE COSTOS AWS MENSUALES**

### **Infraestructura (54.79 EUR)**
- EC2 t2.large: 35.04 EUR
- EBS Storage: 10.50 EUR
- Backup: 5.25 EUR
- Data Transfer: 2.00 EUR
- Monitoring: 2.00 EUR

### **Almacenamiento (23.40 EUR)**
- Landing Bucket (100GB): 2.30 EUR
- Raw Bucket (100GB): 2.30 EUR
- Master Bucket (100GB): 2.30 EUR
- Airflow Bucket (100GB): 2.30 EUR
- Athena Bucket (100GB): 2.30 EUR
- Lifecycle & Analytics: 1.50 EUR

### **Contenedores (18.92 EUR)**
- ECR Repositories: 0.20 EUR
- ECS Tasks Python (100/month): 10.00 EUR
- ECS Tasks Spark (20/month): 8.80 EUR

### **Procesamiento (301.38 EUR)**
- Glue Crawlers: 0.88 EUR
- Athena Queries (100/month): 50.00 EUR
- **Athena Data Scanned (50GB/month): 250.00 EUR** ⚠️

### **Visualización (46.25 EUR)**
- QuickSight (2 usuarios): 18.00 EUR
- SPICE Storage (5GB): 1.25 EUR

### **Redes y Seguridad (45.00 EUR)**
- NAT Gateway: 45.00 EUR
- VPC, Subnets, Security Groups: Gratis

### **Monitoreo (5.80 EUR)**
- CloudWatch Logs (10GB): 5.00 EUR
- Metrics: 0.30 EUR
- Alarms (5): 0.50 EUR

---

## 💰 **ROI Y JUSTIFICACIÓN**

### **Inversión Total Primer Año: 44,382 EUR**
### **Costos Operativos Anuales: 5,942 EUR**
### **Ahorro Anual Estimado: 50,000 EUR**
### **ROI Anual: 113%**
### **Tiempo de Recuperación: 11 meses**

---

## 📊 **COMPARACIÓN CON COSTOS INICIALES**

### **Cambios Realizados:**
- **Volumen S3**: Reducido a 500GB total (bajo volumen inicial)
- **Consultas Athena**: 100/month (bajo volumen)
- **Data Scanned**: 50GB/month (bajo volumen)
- **Usuarios QuickSight**: 2 usuarios iniciales
- **Logs CloudWatch**: 10GB/month (bajo volumen)

### **Impacto en Costos:**
- **Costo Mensual AWS**: Reducido de 2,800 EUR a 495 EUR (82% reducción)
- **Costo Anual AWS**: Reducido de 33,606 EUR a 5,942 EUR (82% reducción)
- **Total Primer Año**: Reducido de 72,046 EUR a 44,382 EUR (38% reducción)

---

*📊 **Nota**: Los costos están basados en estimaciones para un volumen de datos bajo al inicio. Los costos reales pueden variar según el uso y volumen de datos procesados. Se recomienda monitorear el uso y escalar gradualmente según las necesidades.* 