# Aviation Flight Logistics: Time-Series Standardization & Data Integrity

## 1. Contexto del Proyecto
La gestión de datos en la industria aeronáutica requiere una precisión absoluta en las dimensiones temporales y geográficas. Este proyecto se centra en la limpieza de un dataset global de vuelos (`Airline Dataset Updated - v2`) para eliminar inconsistencias en registros de salida y normalizar la identificación de infraestructuras aeroportuarias, permitiendo un análisis de puntualidad (On-Time Performance) confiable.

## 2. Diagnóstico del Dataset Crudo (Capa Bronze)
El dataset original presentaba desafíos que comprometían la integridad referencial:
* **Entropía Temporal:** Fechas de salida con múltiples formatos mezclados (ej. `MM/DD/YYYY` vs `DD-MM-YYYY`), lo que impedía el ordenamiento cronológico y análisis de tendencias.
* **Inconsistencias Geográficas:** Códigos de país y nombres de continentes con variaciones ortográficas o errores de carga manual que fragmentaban las métricas.
* **Ruido en Identificadores:** IATA Codes (códigos de aeropuertos) con espacios en blanco o caracteres especiales que bloqueaban los cruces de tablas (Joins).
* **Duplicidad de Registros:** Presencia de datos redundantes que inflaban artificialmente las métricas de tráfico aéreo.

## 3. Arquitectura de la Solución (Proceso de Transformación)

### A. Estandarización de Series Temporales
Se implementó una lógica de transformación de fechas para unificar todo el dataset bajo el estándar internacional **ISO 8601 (`YYYY-MM-DD`)**:
* Detección de patrones mediante condicionales para separar días de meses en formatos ambiguos.
* Conversión de la columna `Departure Date` a un tipo de dato *Date* real, habilitando el análisis de estacionalidad y series de tiempo.



### B. Validación de Infraestructura (IATA & City Codes)
Se realizó una limpieza profunda de las columnas de localización:
* **Trimming:** Eliminación de espacios innecesarios en `Arrival Airport` y `Airport Name`.
* **Case Normalization:** Estandarización a mayúsculas para asegurar la consistencia en identificadores de aeropuertos.
* **Verificación:** Validación de consistencia entre el código de aeropuerto y el nombre de la ciudad.

### C. Limpieza de Logs de Pasajeros y Estados
* **Deduplicación:** Eliminación de registros duplicados basados en el identificador único `Passenger ID`.
* **Normalización Categórica:** Unificación de la columna `Flight Status` para agrupar categorías inconsistentes en estados claros de negocio: *On Time, Delayed, Cancelled*.



## 4. Impacto en el Negocio
El dataset optimizado (`Airline Dataset Updated - v2 limpio.csv`) funciona como una fuente de verdad para:
1. **Análisis de Puntualidad (OTP):** Cálculo preciso de tasas de retraso por aerolínea y nodo aeroportuario.
2. **Optimización de Rutas:** Identificación de corredores aéreos con mayor volumen de incidencias.
3. **Métricas de Pasajeros:** Segmentación demográfica precisa para estrategias de marketing y lealtad sin sesgos por duplicados.



## 5. Automatización del Pipeline (Python / Pandas)
El procesamiento manual se migró a un entorno de código mediante un script de Python (`02_Automated_Cleaning_Airlines.ipynb`), garantizando la repetibilidad del proceso ante nuevas cargas de datos mensuales.

**Técnicas de Ingeniería Aplicadas:**
* **Parsing Temporal Avanzado:** Uso de `pd.to_datetime()` y `.dt.strftime()` para forzar la estandarización de fechas caóticas al formato corporativo ISO 8601 (`YYYY-MM-DD`).
* **Deduplicación Estricta:** Implementación de `.drop_duplicates(subset=['Passenger ID'])` para auditar y eliminar registros clonados que sesgaban el volumen de tráfico.
* **Normalización de Strings:** Aplicación de métodos en cadena (`.str.strip().str.upper()`) para limpiar espacios ocultos y estandarizar códigos IATA, asegurando la integridad referencial para futuros cruces de tablas (Joins).

---

### Tecnologías Aplicadas
* **Data Cleansing:** Time-series parsing, String manipulation.
* **Quality Assurance:** Deduplication, Logic validation.
* **Environment:** Advanced Data Wrangling.

---

**Nota para el Reclutador:** Este módulo demuestra competencia en el manejo de grandes volúmenes de datos donde la precisión de la cronología es crítica para la toma de decisiones operativa.