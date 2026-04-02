# Real Estate Valuation: Data Cleaning & Address Parsing Strategy

## 1. Contexto del Proyecto
La precisión en los datos inmobiliarios es crítica para la valuación de activos y la toma de decisiones de inversión. Este proyecto consiste en la limpieza y estructuración de un dataset de transacciones de propiedad en Nashville. El objetivo fue transformar registros con alta fragmentación y datos faltantes en una base de datos íntegra capaz de alimentar modelos de predicción de precios y análisis de mercado.

## 2. Diagnóstico del Dataset Crudo (Capa Bronze)
El dataset original (`Nashville Housing sucio.csv`) presentaba obstáculos severos para el análisis:
* **Direcciones No Estructuradas:** Información de calle y ciudad fusionada en una sola cadena, impidiendo segmentar el mercado por zonas geográficas.
* **Valores Nulos en Campos Críticos:** Ausencia de datos en nombres de propietarios (`OwnerName`) y dimensiones de terreno (`Acreage`), lo que sesgaba los promedios de valor.
* **Formatos de Fecha Heterogéneos:** Fechas de venta cargadas como texto, bloqueando el cálculo de apreciación temporal.
* **Inconsistencia Booleana:** Datos en campos como `SoldAsVacant` con mezclas de "Y/N" y "Yes/No", lo que impedía el filtrado binario exacto.

## 3. Arquitectura de la Solución (Proceso de Transformación)

### A. Parsing de Direcciones y Geocodificación Lógica
Se aplicó una técnica de descomposición de cadenas para normalizar la ubicación de las propiedades:
* **Segmentación:** La columna `PropertyAddress` se dividió en `Address` y `City`.
* **Propietarios:** Se procesó la información para extraer `OwnerAddress`, `OwnerCity` y `OwnerState` de forma independiente.
* **Resultado:** Capacidad de realizar análisis geoespaciales y segmentación por distritos fiscales.



### B. Tratamiento de Valores Faltantes (Handling Missing Data)
Se implementó una política de integridad para los valores nulos (NaN/Nulls):
* Para evitar errores en reportes de BI, los campos vacíos en nombres de propietarios se imputaron con el valor **"Sin Datos"**.
* Se realizó una validación cruzada para asegurar que los distritos fiscales (`TaxDistrict`) fueran consistentes con las ciudades extraídas.

### C. Normalización Financiera y Temporal
* **Casting de Fechas:** Conversión de `SaleDate` al estándar internacional `YYYY-MM-DD`, habilitando el análisis de volumen de ventas por periodos.
* **Estandarización de Precios:** Limpieza de la columna `SalePrice` para asegurar su tratamiento como valor numérico continuo, eliminando caracteres no deseados.
* **Mapeo de Variables:** Unificación de criterios en `SoldAsVacant` hacia un formato binario estándar (Yes/No).



## 4. Impacto en el Negocio
El dataset final (`Nashville Housing limpio.csv`) es un activo listo para:
1. **Análisis de Plusvalía:** Evaluación de la evolución de precios por ciudad y año de construcción.
2. **Segmentación de Inversores:** Identificación de patrones de compra según el estado de procedencia de los propietarios.
3. **Modelado Predictivo:** Datos listos para entrenar algoritmos que predigan el `TotalValue` en función de variables como `Bedrooms`, `FullBath` y `Acreage`.

---

### Tecnologías Aplicadas
* **Data Wrangling:** Column splitting, Null handling, Value mapping.
* **Financial Integrity:** Price normalization, Date casting.
* **Environment:** Professional Data Cleansing Workflow.

---

**Nota para el Reclutador:** Este proyecto demuestra una capacidad avanzada para manejar datos de activos complejos, asegurando que la geolocalización y el valor financiero sean consistentes para el análisis de inversión de alto nivel.