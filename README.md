# 📈 E-commerce Profitability & Data Engineering: Microsoft Fabric Architecture

Este proyecto implementa una solución de **Ingeniería de Datos de extremo a extremo** utilizando **Microsoft Fabric**. El objetivo principal es transformar datos transaccionales ruidosos en una arquitectura de **Lakehouse** optimizada para el cálculo de la rentabilidad real (Net Profit) y la eficiencia operativa mediante un enfoque de **Arquitectura Medallion**.

## 🎯 El Problema de Negocio (Business Case)
Las organizaciones de E-commerce suelen operar con una visión parcial de su salud financiera debido a:
* **Datos Fragmentados:** Información dispersa entre Shopify/Amazon, ERPs y operadores logísticos.
* **Inconsistencia de Tipos:** Datos numéricos que ingresan como texto (`String`), bloqueando cualquier análisis de agregación.
* **Costos Ocultos:** Incapacidad de integrar devoluciones, comisiones de pasarelas y gastos de última milla en el cálculo del margen bruto y neto.

## 🏗️ Arquitectura de Datos (Modern ELT)
A diferencia del ETL tradicional, se ha implementado un flujo **ELT** (Extract, Load, Transform) aprovechando el poder de procesamiento de **Microsoft Fabric** y el almacenamiento unificado en **OneLake**.

<img width="1904" height="899" alt="image" src="https://github.com/user-attachments/assets/22b43c7a-f93e-49fd-b31a-cd59e7f535be" />

<img width="1917" height="746" alt="image" src="https://github.com/user-attachments/assets/a274f752-a44e-4f56-b562-bbe40d112c74" />


### Capas del Lakehouse:
1.  **Capa Bronze (Raw):** Ingesta de archivos CSV, Excel y conexiones SQL mediante **Data Factory Pipelines**. Los datos se mantienen en su formato original para auditoría.
2.  **Capa Silver (Cleansed):** Procesamiento de datos con **Power Query Online**. 
    * **Solución al Reto Técnico:** Limpieza de símbolos de moneda y transformación de tipos `String` a `Decimal`.
    * Normalización de esquemas y eliminación de duplicados.
3.  **Capa Gold (Curated):** Creación de un **Modelo en Estrella (Star Schema)**. Los datos se sirven mediante **Direct Lake**, permitiendo que Power BI consulte archivos Parquet sin necesidad de importar datos, garantizando latencia mínima.

## 📈 Modelo de Datos Optimizado
El diseño del modelo se creo utilizando una tabla de hechos de ventas y dimensiones de producto, tiempo, geografía y canales.

<img width="1893" height="862" alt="image" src="https://github.com/user-attachments/assets/427ef38f-2693-4fab-839a-964ebd3fb882" />

> **Solución de Ingeniería:** Durante la transformación en la capa **Silver**, se implementó un script que utiliza funciones de reemplazo para caracteres no numéricos y un re-tipado forzado al esquema de datos. Esto aseguró que el motor de Power BI pudiera ejecutar medidas DAX de inteligencia de tiempo y cálculos de margen sin errores de compatibilidad.

## ⚙️ Tecnologías Utilizadas
* **Microsoft Fabric:** Orquestación, Lakehouse y Gobernanza.
* **OneLake:** Almacenamiento en formato **Delta / Parquet**.
* **Power BI & DAX:** Modelado semántico y visualización de KPIs.
* **Power Query (M):** Motores de transformación para la limpieza de datos.
* **n8n / Python:** Automatización de orquestación externa.

## 📂 Estructura del Proyecto
```text
├── PBIs/
│   └── Ecomm_Profitability_Model.pbix   # Archivo fuente de Power BI
├── Transformaciones/
│   └── silver_layer_cleaning.m          # Scripts de limpieza en M
├── img/                                # Capturas de pantalla y diagramas
└── README.md                           # Documentación técnica



