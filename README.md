Data Cleaning & ETL Architecture Portfolio
📌 Visión General
Este repositorio centraliza una serie de proyectos de Ingeniería de Datos y Limpieza Avanzada (ETL). El objetivo es demostrar la capacidad de transformar datasets de alta entropía (Capa Bronze) en activos de datos gobernados y listos para la producción (Capa Gold), aplicando estándares de industria para asegurar la integridad y veracidad de la información.

🛠️ Stack Tecnológico
Limpieza y Normalización: Regular Expressions (REGEX), Data Parsing, Unit Standardization.

Modelado: Dimensional Modeling, Relational Joins, Feature Engineering.

Herramientas: Google Sheets/Excel (Advanced), preparando la transición hacia pipelines automatizados en Python (Pandas) y SQL (BigQuery).

📂 Estructura del Repositorio
El portfolio está organizado por verticales de negocio, cada una con su propia documentación técnica detallada:

1. Tech Hardware Pricing
Enfoque: Normalización de especificaciones técnicas y escalabilidad de unidades.

Desafío: Extraer capacidades de almacenamiento híbridas y frecuencias de CPU de cadenas de texto no estructuradas.

Resultado: Dataset listo para modelos de regresión de precios.

2. Aviation Flight Logistics
Enfoque: Estandarización de Series Temporales y consistencia de datos geográficos.

Desafío: Corregir formatos de fecha inconsistentes y validar códigos de aeropuertos internacionales.

Resultado: Registro histórico optimizado para análisis de puntualidad (On-Time Performance).

3. Real_Estate_Valuation
Enfoque: Limpieza profunda de Nulos y Parsing de direcciones.

Desafío: Manejo de valores faltantes en propiedades de alto valor y fragmentación de datos de ubicación.

Resultado: Base de datos depurada para valuación de mercado y análisis de inversión.

📈 Metodología de Trabajo: "Medallion Architecture"
En cada proyecto se respetó el flujo de datos profesional:

Bronze (Raw): El dato tal cual llega de la fuente, con errores, nulos y formatos inconsistentes.

Silver (Cleaned): Datos tipados, sin duplicados y con unidades normalizadas.

Gold (Curated): Datos enriquecidos con tablas de dimensiones, listos para BI o Machine Learning.

👤 Contacto
Estudiante de Data Analysis enfocado en la automatización de procesos y creación de valor a través del dato.

Meta 2026: Transición completa hacia flujos de trabajo automatizados con Python y dbt.