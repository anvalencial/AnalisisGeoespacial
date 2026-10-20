# Datos del proyecto: susceptibilidad a represamientos por movimientos en masa

Proyecto del curso Análisis Geoespacial (Maestría en Geomorfología y Suelos, Universidad Nacional de Colombia, sede Medellín).
Ana María Valencia Londoño.

Esta carpeta reúne los **datos de entrada** del proyecto, todos de fuentes públicas. El área de estudio es una zona de 70 523 km² en el norte de los Andes colombianos (Antioquia y alrededores).

**Sistema de coordenadas:** todas las capas están en WGS 84 / UTM zona 19N (EPSG:32619), excepto CHIRPS, que está en WGS 84 geográficas (EPSG:4326).

## Puntos (`puntos/`)

| Archivo | Capa | Elementos | Descripción | Fuente |
|---|---|---|---|---|
| `movimientos_en_masa.gpkg` | `simma_inventario` | 1914 | Inventario de movimientos en masa del SIMMA | Servicio Geológico Colombiano (SGC), SIMMA |
| | `simma_catalogo` | 11 743 | Catálogo de movimientos en masa del SIMMA | SGC, SIMMA |
| | `geohazards` | 1305 | Movimientos en masa y avenidas torrenciales | Base de datos Geohazard (Aristizábal et al., 2025) |
| | `eventos_unidos` | 3038 | Unión depurada de las fuentes anteriores dentro del área | Elaboración propia a partir de SIMMA y Geohazard |
| `sismos.gpkg` | `sismos_sgc` | 687 | Sismos del catálogo nacional | SGC, Red Sismológica Nacional |
| | `sismos_usgs` | 505 | Sismos del catálogo mundial | USGS, servicio FDSN |
| | `sismos_unidos` | 823 | Unión de los dos catálogos sin duplicados | Elaboración propia |
| | `pga475_cabeceras` | 332 | Aceleración pico del suelo para 475 años de periodo de retorno en cabeceras municipales | SGC, Modelo Nacional de Amenaza Sísmica |

## Líneas (`lineas/`)

| Archivo | Elementos | Descripción | Fuente |
|---|---|---|---|
| `drenajes.gpkg` | 12 309 | Red de drenaje de orden de Strahler ≥ 3, con el campo `Orden` y la longitud en km | Derivada del DEM ALOS PALSAR de 12,5 m: dirección de flujo D8, umbral de 999 celdas (0,156 km²) y orden de Strahler |
| `fallas_atlas2020.gpkg` | 1223 | Fallas geológicas | SGC, Atlas Geológico de Colombia 2020 |
| `vias_igac.gpkg` | 1207 | Vías a escala 1:500 000 | Instituto Geográfico Agustín Codazzi (IGAC) |

## Polígonos (`poligonos/`)

| Archivo | Elementos | Descripción | Fuente |
|---|---|---|---|
| `area_estudio.gpkg` | 1 | Polígono del área de estudio (70 523 km²) | Elaboración propia |
| `geologia_atlas2020.gpkg` | 1495 | Unidades cronoestratigráficas | SGC, Atlas Geológico de Colombia 2020 |

## Raster (`raster/`)

| Archivo | Resolución | Descripción | Fuente |
|---|---|---|---|
| `dem_alos_100m.tif` | 100 m | Modelo digital de elevación remuestreado a 100 m (promedio de 8 × 8 celdas), recortado al área de estudio. Valores en metros (entero), sin dato = −32768 | ALOS PALSAR RTC de 12,5 m, ASF DAAC. Includes material © JAXA/METI 2007 |
| `chirps_1981_2024.tif` | 0,05° (~5,5 km) | Precipitación anual 1981–2024. Banda 1: media anual (mm); banda 2: máximo anual (mm); banda 3: coeficiente de variación (%) | CHIRPS v2.0, Climate Hazards Center, UC Santa Barbara |

**DEM original.** El DEM de 12,5 m pesa unos 670 MB y supera el límite de GitHub (100 MB por archivo), así que no está en el repositorio. Se descarga de Alaska Satellite Facility (https://search.asf.alaska.edu), producto ALOS PALSAR *Radiometric Terrain Corrected* de alta resolución.

## Cómo abrir los datos

Los archivos `.gpkg` (GeoPackage) se abren directamente en ArcGIS Pro, QGIS o Python. Un mismo `.gpkg` puede tener varias capas:

```python
import geopandas as gpd
import pyogrio

pyogrio.list_layers("puntos/movimientos_en_masa.gpkg")      # ver las capas
eventos = gpd.read_file("puntos/movimientos_en_masa.gpkg", layer="eventos_unidos")
```

## Referencias de los datos

- Aristizábal, E., Correa, D., García Cadavid, C., Valencia Londoño, A. M. y Zabaleta Benavides, E. A. (2025). Geohazard: base de datos por movimientos en masa y avenidas torrenciales en Colombia y el departamento de Antioquia. *Ciencia e Ingeniería Neogranadina, 35*(1), 131–147. https://doi.org/10.18359/rcin.7786
- ASF DAAC (2015). ALOS PALSAR Radiometric Terrain Corrected high res. Includes material © JAXA/METI 2007. https://doi.org/10.5067/Z97HFCNKR6VA
- Funk, C. et al. (2015). The climate hazards infrared precipitation with stations — a new environmental record for monitoring extremes. *Scientific Data, 2*, 150066. https://doi.org/10.1038/sdata.2015.66
- Servicio Geológico Colombiano. Sistema de Información de Movimientos en Masa (SIMMA). https://simma.sgc.gov.co
- Servicio Geológico Colombiano (2020). Atlas Geológico de Colombia 2020.
- Instituto Geográfico Agustín Codazzi. Cartografía básica 1:500 000.
