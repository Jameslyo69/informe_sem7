# Laboratorio 7: Pipeline ETL con Capa de Cuarentena en un Data Lake

**Materia:** Fundamentos de Big Data (IDSD413), Escuela Politécnica Nacional
**Autor:** James Caiño
**Modalidad:** Trabajo individual

## Descripción
En este repositorio se encuentra el notebook de Google Colab en el que intervine un pipeline
ETL que procesa 5.050.000 transacciones sintéticas con valores nulos, registros duplicados y
valores atípicos. Sobre el notebook base implementé dos retos de ingeniería de datos y
organicé el resultado en un Data Lake de tres capas.

## Arquitectura del Data Lake

| Capa | Ruta | Contenido |
|---|---|---|
| Raw | `./datalake/raw` | Dato crudo e inmutable, tal como se ingirió |
| Quarantine | `./datalake/quarantine` | Outliers aislados con metadatos para auditoría |
| Curated | `./datalake/curated` | Dato limpio y normalizado, listo para el modelado |

## Retos resueltos

**Reto 1: Capa de Cuarentena.** Los outliers detectados con la regla IQR ya no se eliminan.
Se separan del flujo principal y se guardan en formato Parquet en `./datalake/quarantine`,
junto con el motivo, los límites IQR utilizados y la fecha de cuarentena, porque pueden
corresponder a fraudes o a errores críticos del sistema origen. Un control de integridad
verifica que ningún registro se pierda.

**Reto 2: Descarte Inteligente.** Las transacciones con estado `Cancelado` y monto nulo se
eliminan, ya que no aportan valor predictivo. Para `Completado`, `Pendiente` y `Reembolsado`
se mantiene la imputación por mediana por categoría.

## Resultados

| Etapa | Registros |
|---|---|
| Ingesta en la capa Raw | 5.050.000 |
| Duplicados eliminados | 49.999 |
| Cancelados con monto nulo eliminados | 62.513 |
| Nulos imputados por mediana | 187.479 |
| Outliers enviados a Quarantine | 101 |
| Registros en la capa Curated | 4.937.387 |

La desviación estándar del monto pasó de 45.882,62 en el dato crudo a 706,46 en el dato curado.

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| `S7_ETL_Y_DATA_LAKE_EN_GOOGLE_COLAB_...ipynb` | Notebook de Google Colab con la solución de los dos retos |
| `README.md` | Descripción del proyecto |

## Cómo ejecutar
1. Abrir el notebook en Google Colab.
2. Ejecutar todas las celdas en orden (*Entorno de ejecución → Ejecutar todas*).
3. Revisar la carpeta `datalake/` en el explorador de archivos de Colab, donde aparecen las
   capas `raw`, `curated` y `quarantine` con sus archivos Parquet.

## Tecnologías
Python 3, pandas, NumPy, PyArrow (Parquet) y Google Colab.
