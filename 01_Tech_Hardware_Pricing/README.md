# Tech Hardware Pricing: ETL & Data Normalization Pipeline

## 1. Contexto del Proyecto
En el sector de Retail Tecnológico, la dispersión en los formatos de entrada de datos (logs de inventario manuales) impide la ejecución de modelos de precios comparativos. Este proyecto aborda la transformación de un dataset crudo de laptops, caracterizado por una alta entropía en las especificaciones técnicas, hacia un **Golden Record** estructurado para el análisis de BI.

## 2. Diagnóstico del Dataset Crudo (Capa Bronze)
El archivo original (`laptopData sucio .csv`) presentaba anomalías críticas que bloqueaban cualquier proceso de agregación:
* **Unidades de Medida Incrustadas:** Valores numéricos (RAM, Peso, Frecuencia) almacenados como texto (ej. "1.37kg", "8GB"), inhabilitando cálculos matemáticos.
* **Estructuras de Almacenamiento No Normalizadas:** Celdas con configuraciones duales (SSD + HDD) que impedían el cálculo de la capacidad real total.
* **Falta de Dimensionalidad Geográfica:** Datos limitados a la marca, sin contexto sobre el origen de fabricación o logística.
* **Inconsistencia en Frecuencias de CPU:** Velocidades de procesamiento secuestradas dentro de cadenas de texto complejas.

## 3. Arquitectura de la Solución (Proceso de Transformación)

Para alcanzar la integridad de los datos, se implementó un pipeline de limpieza basado en cuatro pilares:

### A. Normalización de Unidades y Tipado
Se aplicaron técnicas de extracción para aislar magnitudes numéricas de sus unidades:
* **Peso y RAM:** Eliminación de sufijos y conversión a tipos `float/int`.
* **Precios:** Estandarización a moneda entera (`Price_INR`) eliminando ruido decimal excesivo para reportes financieros.



### B. Desagregación de Almacenamiento (Feature Engineering)
Dada la complejidad de las configuraciones híbridas, se hacheó la información mediante lógica de separación (`SPLIT`) y expresiones regulares (`REGEX`):
* Se crearon columnas independientes para el **Disco 1** y **Disco 2**.
* Se normalizaron las capacidades a una unidad común (GB), convirtiendo valores de Terabytes (TB) mediante factores de escala (x1000).
* **Resultado:** Una columna de `Capacidad_Total_GB` que permite realizar análisis de correlación directos entre almacenamiento y precio.

### C. Enriquecimiento Relacional
Mediante el uso de un diccionario de dimensiones externo (`Diccionario_Marcas.csv`), se inyectó la procedencia geográfica (`Country`) a cada registro. Esto transforma una tabla de productos en una base de datos con capacidad de análisis por regiones económicas (USA, China, Taiwán, etc.).

### D. Extracción de Frecuencia de CPU (REGEX Pattern Matching)
Se diseñó un patrón de captura para identificar frecuencias de reloj (GHz) ignorando el modelo de procesador, permitiendo segmentar el rendimiento de la CPU como una variable cuantitativa continua.



## 4. Impacto en el Negocio
El dataset final (`laptopData limpio .csv`) habilita inmediatamente:
1. **Análisis de Margen por País:** Identificar la rentabilidad según el origen de la marca.
2. **Modelos de Elasticidad de Precio:** Evaluar cuánto incrementa el precio por cada GB o GHz adicional de forma precisa.
3. **Optimización de Stock:** Clasificación clara por tipo de formato (Ultrabook, Notebook, etc.) y tecnología de disco (SSD vs HDD).



## 5. Automatización del Pipeline (Python / Pandas)
Para escalar esta solución y abandonar el procesamiento manual en hojas de cálculo, se desarrolló un script de automatización en Python (`01_Automated_Cleaning_Laptops.ipynb`). 

**Técnicas de Ingeniería Aplicadas:**
* **Extracción Regex:** Uso de `.str.extract('(\d+)')` para aislar valores numéricos de cadenas de texto complejas (ej. transformar "128GB SSD" a `128.0`).
* **Lógica Condicional Vectorizada:** Implementación de `.loc[]` para detectar discos en Terabytes y multiplicar su valor por 1000 dinámicamente.
* **Manejo de Nulos en Cálculos:** Uso de `.fillna(0)` durante la suma de capacidades de almacenamiento para evitar la propagación de valores `NaN` en laptops con un solo disco.
* **Casting de Datos:** Conversión de tipos `object` a `float` e `int` para habilitar operaciones matemáticas.

---

### Tecnologías Aplicadas
* **Data Cleaning:** Regex, Split-Apply-Combine logic.
* **Data Modeling:** Dimensional Enrichment (Left Join logic).
* **Environment:** Advanced Spreadsheet Engineering (Pre-Python scripting phase).

---

**Nota para el Reclutador:** Este proyecto demuestra la capacidad de transformar datos ruidosos en estructuras de datos gobernadas, asegurando que la calidad del dato preceda a la visualización.