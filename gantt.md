# Cronograma del Proyecto Data Lake AWS

## Diagrama de Gantt del Proyecto

```mermaid
gantt
    title Cronograma del Proyecto Data Lake AWS
    dateFormat  YYYY-MM-DD
    axisFormat %d/%m
    
    section 🚀 Inicio del Proyecto
    Inicio del Proyecto                  :milestone, inicio, 2025-08-25, 0d
    
    section 🏗️ Fase 1: Infraestructura Base
    Configuración AWS                    :aws, 2025-08-25, 2025-09-07
    Construcción Data Lake              :datalake, 2025-08-25, 2025-09-14
    Arquitectura de Almacenamiento      :arch, 2025-09-08, 2025-09-21
    
    section 🔄 Fase 2: Desarrollo ETL
    Monitoreo de Emails                 :etl1, 2025-09-22, 2025-09-28
    Extracción de Attachments           :etl2, 2025-09-22, 2025-09-30
    Ingesta en Data Lake               :etl3, 2025-09-25, 2025-10-02
    Transformación de Datos             :etl4, 2025-09-28, 2025-10-05
    Catalogación                        :etl5, 2025-10-01, 2025-10-05
    
    section 🧪 Fase 3: Pruebas y Dashboard
    Pruebas Finales                     :test, 2025-10-06, 2025-10-12
    Ajustes del Sistema                 :test, 2025-10-08, 2025-10-15
    Creación Dashboard QuickSight       :dashboard, 2025-10-10, 2025-10-17
    Despliegue Final                    :deploy, 2025-10-15, 2025-10-19
```

## 📅 Cronograma Detallado por Semanas

### **Semana 1-2 (25 Ago - 7 Sep 2025)**
- **Configuración AWS**: Configuración de cuentas, IAM, VPC, y servicios base
- **Construcción Data Lake**: Implementación de buckets S3 (Landing, Raw, Master)

### **Semana 3-4 (8 Sep - 21 Sep 2025)**
- **Arquitectura de Almacenamiento**: Definición de estructura de datos y políticas de retención
- **Finalización Fase 1**: Infraestructura completamente configurada

### **Semana 5-6 (22 Sep - 5 Oct 2025)**
- **Monitoreo de Emails**: Implementación del servicio de monitoreo de correos electrónicos
- **Extracción de Attachments**: Desarrollo del proceso de descarga de archivos Excel
- **Ingesta en Data Lake**: Implementación del proceso de carga inicial en S3 Landing
- **Transformación de Datos**: Desarrollo del pipeline Spark para conversión a formato Parquet
- **Catalogación**: Implementación del crawler de AWS Glue para metadatos

### **Semana 7-8 (6 Oct - 19 Oct 2025)**
- **Pruebas Finales**: Validación completa del flujo ETL
- **Ajustes del Sistema**: Optimizaciones y correcciones basadas en pruebas
- **Creación Dashboard QuickSight**: Desarrollo de visualizaciones y reportes
- **Despliegue Final**: Puesta en producción del sistema completo

## Flujo de Datos ETL

```mermaid
sequenceDiagram
    participant Email as 📧 Email Service
    participant Airflow as ⚡ Apache Airflow
    participant Python as 🐍 Python Container
    participant Spark as 🔥 Spark Container
    participant Landing as 📥 Landing S3
    participant Raw as 🔄 Raw S3
    participant Master as 🎯 Master S3
    participant Glue as 🕷️ AWS Glue
    participant Athena as 🏛️ Athena
    participant QuickSight as 📊 QuickSight
    
    Note over Email,QuickSight: 🚀 FLUJO ETL COMPLETO
    
    Email->>Airflow: Nuevo email con Excel
    Airflow->>Python: Ejecutar script de monitoreo
    Python->>Email: Descargar attachment
    Python->>Landing: Guardar Excel original
    
    Airflow->>Spark: Ejecutar transformación
    Spark->>Landing: Leer Excel
    Spark->>Raw: Guardar datos en Parquet
    
    Airflow->>Spark: Ejecutar modelado
    Spark->>Raw: Leer datos raw
    Spark->>Master: Guardar tablón final
    
    Airflow->>Glue: Ejecutar crawler
    Glue->>Landing: Catalogar landing
    Glue->>Raw: Catalogar raw
    Glue->>Master: Catalogar master
    
    Athena->>Glue: Consultar metadatos
    Athena->>Master: Ejecutar consultas SQL
    
    QuickSight->>Athena: Obtener datos
    QuickSight->>QuickSight: Generar dashboards
```

## 🎯 Hitos del Proyecto

| Hito | Fecha Objetivo | Descripción |
|------|----------------|-------------|
| 🏆 **Hito 1** | 21 Sep 2025 | Infraestructura AWS completamente configurada |
| 🚀 **Hito 2** | 5 Oct 2025 | Pipeline ETL funcionando y procesando datos |
| 🎉 **Hito 3** | 19 Oct 2025 | Sistema completo en producción con dashboard activo |

## 👥 Recursos y Responsabilidades

- **👨‍💻 DevOps Engineer**: Configuración AWS, infraestructura
- **🔧 Data Engineer**: Desarrollo ETL, pipeline de datos
- **📊 Data Analyst**: Creación de dashboard, validación de datos
- **📋 Project Manager**: Coordinación general, seguimiento de cronograma

## 📅 Resumen de Cronograma

- **🚀 Fecha de Inicio**: Lunes 25 de Agosto de 2025
- **🏁 Fecha de Finalización**: Domingo 19 de Octubre de 2025
- **⏱️ Duración Total**: 8 semanas (2 meses)
- **📋 Fases**: 3 fases principales con hitos claros cada 2-4 semanas

## 🎨 Características del Cronograma

- **🔄 Paralelización**: Algunas tareas se ejecutan en paralelo para optimizar tiempo
- **📈 Dependencias**: Las fases tienen dependencias lógicas entre sí
- **🎯 Milestones**: Hitos claros para seguimiento del progreso
- **⚡ Agilidad**: Metodología ágil con entregas incrementales

## 📊 Cronograma Semanal Detallado

| Semana | Fechas | Actividades Principales | Entregables |
|--------|--------|------------------------|-------------|
| **1** | 25-31 Ago | Configuración AWS inicial | Cuentas y servicios base configurados |
| **2** | 1-7 Sep | Construcción Data Lake | Buckets S3 creados y configurados |
| **3** | 8-14 Sep | Arquitectura de almacenamiento | Estructura de datos definida |
| **4** | 15-21 Sep | Finalización infraestructura | Infraestructura lista para ETL |
| **5** | 22-28 Sep | Desarrollo ETL inicial | Monitoreo de emails funcionando |
| **6** | 29 Sep-5 Oct | Finalización ETL | Pipeline completo funcionando |
| **7** | 6-12 Oct | Pruebas y ajustes | Sistema validado y optimizado |
| **8** | 13-19 Oct | Dashboard y despliegue | Sistema en producción |

## 🔍 Nota Importante sobre el Gantt

El diagrama de Gantt de Mermaid puede cortar automáticamente el eje temporal para optimizar la visualización. Para asegurar que se vea la fecha de inicio completa (25 de Agosto 2025), se ha agregado un **milestone de inicio** que fuerza a Mermaid a mostrar desde esa fecha.

**Fechas clave del proyecto:**
- **🚀 INICIO**: 25 de Agosto de 2025
- **🏁 FIN**: 19 de Octubre de 2025
- **⏱️ DURACIÓN**: 8 semanas exactas