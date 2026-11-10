# Laboratorio 7: Pipeline ETL con Capa de Cuarentena en un Data Lake

**Materia:** Fundamentos de Big Data (IDSD413), Escuela Politécnica Nacional
**Autor:** Tu Nombre Completo

## Descripción
Extensión de un pipeline ETL en Google Colab que procesa 5,05 millones de transacciones
sintéticas con nulos, duplicados y outliers, organizadas en un Data Lake de tres capas.

## Arquitectura del Data Lake
| Capa | Ruta | Contenido |
|---|---|---|
| Raw | `./datalake/raw` | Dato crudo e inmutable tal como se ingirió |
| Quarantine | `./datalake/quarantine` | Outliers aislados con metadatos para auditoría |
| Curated | `./datalake/curated` | Dato limpio y normalizado, listo para Machine Learning |

## Retos resueltos
**Reto 1: Capa de Cuarentena.** Los outliers detectados con la regla IQR no se eliminan:
se guardan en formato Parquet en `./datalake/quarantine` junto con el motivo, los límites
IQR y la fecha, porque pueden representar fraude o errores críticos del origen.

**Reto 2: Descarte Inteligente.** Las transacciones `Cancelado` con monto nulo se eliminan,
ya que no aportan valor predictivo. Para `Completado`, `Pendiente` y `Reembolsado` se
mantiene la imputación por mediana por categoría.

## Resultados
| Etapa | Registros |
|---|---|
| Ingesta (Raw) | 5.050.000 |
| Duplicados eliminados | 49.999 |
| Cancelados nulos eliminados | 62.513 |
| Nulos imputados | 187.479 |
| Outliers en cuarentena | 101 |
| Registros en Curated | 4.937.387 |

## Estructura del repositorio
