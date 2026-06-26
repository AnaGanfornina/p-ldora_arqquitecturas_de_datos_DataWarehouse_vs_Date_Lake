# Arquitecturas de Datos: Data Lake vs. Data Warehouse

Este documento ofrece un resumen completo de la píldora informativa sobre **Arquitecturas de Datos**, analizando el fundamento de una estrategia moderna, la evolución de los diferentes modelos (Data Warehouse, Data Mart, Data Lake y Data Lakehouse), sus ventajas, limitaciones y roles en la era tecnológica actual.

---

## ¿Por qué necesitamos una Arquitectura de Datos?

Cualquier estrategia de datos moderna se fundamenta en responder a tres necesidades críticas de negocio:

1. **Consolidar fuentes:** Ordenar y unificar el gran número de fuentes de datos de origen dispares en un único ecosistema coherente.
2. **Extraer información:** Transformar los datos brutos en *insights* accionables que el negocio pueda leer, interpretar y explotar.
3. **Tomar mejores decisiones:** Disponer de datos fiables, actualizados y accesibles para fundamentar decisiones estratégicas con evidencia.

Cada una de las arquitecturas existentes responde a estas tres necesidades de una forma diferente.

---

## 1. Data Warehouse (DWH)
*El pilar del BI Tradicional*

### Definición
Central de datos integrada, histórica y no volátil. Almacena información altamente estructurada en tablas optimizadas para el análisis semántico.

### Origen e Historia
Surgió a finales de los años 80. Los equipos de Business Intelligence (BI) ponían en peligro la estabilidad operativa al lanzar reportes pesados directamente sobre las bases de datos de producción. El DWH solucionó esto creando un entorno analítico completamente separado.

### Enfoques de la Arquitectura
* **Enfoque de Negocio:** Centrado en el BI clásico (reportes y dashboards para mirar al pasado). El Data Science solía quedar fuera debido a la rigidez de los formatos estructurados.
* **Arquitectura Organizacional (El proceso de negocio):**
  1. *Separación analítica:* Mueve los datos a un sistema físico separado para quitar carga a producción y aumentar el rendimiento analítico.
  2. *Centralización ETL:* Procesos de Extracción, Transformación (limpieza + lógica de negocio) y Carga para estructurar el modelo de datos.
* **Arquitectura Técnica (La infraestructura MPP):**
  1. *MPP (Procesamiento Paralelo Masivo):* Optimizado para escanear grandes volúmenes de datos en paralelo con SQL de alto rendimiento.
  2. *Almacenamiento Columnar + ELT:* Migración del almacenamiento de filas a columnas para agilizar consultas masivas rápidas. En entornos Cloud, el flujo tradicional se invierte a **ELT** (Cargar primero, transformar después en una zona de *Staging*).

### Ejemplos tecnológicos
Snowflake, Google BigQuery, Amazon Redshift.

---

## 2. Data Mart
*Especialización para analistas de BI*

### Definición
Subconjunto especializado y refinado del Data Warehouse central. Nace debido a que los DWH corporativos se volvieron gigantescos e intimidantes; por ejemplo, un analista de Marketing no necesita visualizar tablas complejas de Recursos Humanos.

### Características Clave
* **¿Por qué surge?:** Aporta agilidad al proveer datos pre-agregados listos para consumir, con mayor rendimiento que consultando el DWH completo.
* **Procesamiento:** Representa la fase final del BI, aplicando transformaciones menores y *joins* complejos a partir del DWH global.
* **Ejemplo actual:** El área de Finanzas utilizando un espacio aislado en Snowflake conectado de forma directa a un tablero de Power BI.

---

## 3. Data Lake
*El nacimiento del Data Science masivo*

> **Filosofía:** *"Guarda todo en bruto hoy, ya descubriremos su valor mañana."*

### Definición
Gran repositorio centralizado diseñado para almacenar datos estructurados, semiestructurados (JSON) y no estructurados (imágenes, audio, texto libre) a escala masiva y a muy bajo coste.

### Características Clave
* **Proceso ELT (Schema-on-Read):** Extracción $
ightarrow$ Carga $
ightarrow$ Transformación. Los datos se guardan sin modificar y la transformación ocurre únicamente al momento del análisis (frecuentemente utilizando herramientas como Apache Spark).
* **Ventajas:** Es masivo, barato, soporta cualquier tipo de dato y es ideal para modelos de Machine Learning (ML).
* **Desventajas:** Riesgo elevado de convertirse en un "pantano de datos" (*Data Swamp*), complejo de operar, lento para consultas SQL tradicionales y difícil de adaptar a normativas de privacidad como el GDPR.
* **Rol actual:** Orientado a mirar al futuro (Data Science). Actúa como un laboratorio perfecto pero a menudo aislado de la realidad inmediata del negocio.

---

## 4. Data Lakehouse
*Borrando la frontera entre BI y Data Science*

### Definición
Una arquitectura híbrida de última generación que implementa las capacidades de **transacciones ACID** y la gobernanza propias de un Data Warehouse directamente sobre el almacenamiento barato y flexible de un Data Lake.

### Características Clave
* **¿Por qué surge?:** Mantener un DWH y un Data Lake en paralelo duplicaba costes, generaba silos de información y derivaba en "pantanos de datos". Además, cumplir con normativas GDPR era prácticamente imposible en un Data Lake puro.
* **Ventajas principales:** Unifica los mundos de BI y Data Science en una sola plataforma, ofrece compatibilidad nativa con GDPR y proporciona control de calidad sobre infraestructura económica.
* **Plataformas de referencia:** Databricks, Microsoft Fabric.
* **Rol actual:** Representa el presente y el futuro de los datos; una plataforma unificada para analistas de negocio y científicos de datos de manera conjunta.

---

## Tabla Comparativa Resumen

| Arquitectura | Ventajas clave | Desventajas | Rol actual |
| :--- | :--- | :--- | :--- |
| **Data Warehouse** | SQL ultra rápido, alta calidad, excelente gobernanza, fácil para BI. | Rígido ante cambios, no soporta datos no estructurados, almacenamiento costoso. | **Mirar al pasado (BI):** Excelente para reportes, pero ciego ante Machine Learning. |
| **Data Lake** | Masivo y barato, soporta cualquier tipo de datos, ideal para ML. | Riesgo de pantano de datos, complejo de operar, lento para SQL, GDPR difícil. | **Mirar al futuro (Data Science):** Laboratorio perfecto, pero aislado del negocio. |
| **Data Mart** | Consultas instantáneas, datos pre-agregados, fácil para usuarios de negocio. | Visión parcial, alta dependencia del DWH central (no es independiente). | **Optimización BI departamental:** Capa de consumo rápido para analistas específicos. |
| **Data Lakehouse** | Unifica BI y Data Science, transacciones ACID sobre almacenamiento barato, compatible con GDPR. | Tecnología reciente, requiere alto *expertise* técnico, ecosistema en maduración. | **El presente y futuro:** Plataforma unificada y colaborativa para analistas y científicos de datos. |
