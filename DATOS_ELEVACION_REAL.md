# Datos de elevación real: perfil longitudinal, corredor Ruta 65 (Casanare)

Fecha de consulta: 2026-09-26. Todas las cifras vienen de consultas reales a APIs públicas. Ninguna es estimada.
Scripts y datos crudos: `scratchpad/elev/` (`run.py`, `prof.py`, `gen2.py`, `points.json`, `profile.json`, `route_*.json`).

## 1. Coordenadas de cabecera usadas

| Código | Municipio   | Lat        | Lon         | Fuente |
|--------|-------------|------------|-------------|--------|
| 85001  | Yopal       | 5.3356662  | -72.3936931 | OSM Nominatim, "Perímetro Urbano Yopal" |
| 85010  | Aguazul     | 5.1708408  | -72.5508460 | OSM Nominatim |
| 85410  | Tauramena   | 5.0165244  | -72.7465858 | OSM Nominatim (place=town) |
| 85162  | Monterrey   | 4.8762611  | -72.8968843 | OSM Nominatim |
| 85300  | Sabanalarga | 4.8564547  | -73.0390642 | OSM Nominatim |
| 85440  | Villanueva  | 4.6147866  | -72.9253833 | OSM Nominatim |

Consulta: `https://nominatim.openstreetmap.org/search?q=<Municipio>,+Casanare,+Colombia&format=json`

**Advertencia sobre el shapefile del proyecto.** `06_TRANSPORTE/NODOS_MUNICIPALES_Casanare6_v2.shp` usa los puntos del servicio DANE
`geoportal.dane.gov.co/mparcgis/rest/services/Divipola/Serv_PuntosCentrosPoblados2013I/MapServer/0`. Esos puntos NO caen sobre el casco urbano.
Por ejemplo, Tauramena aparece en 4.697 N, -72.629 W, y el servicio devuelve exactamente el mismo punto para "PASO CUSIANA" (centro poblado).
Yopal aparece en 5.2425 N, -72.2575 W, unos 17 km al SE del casco urbano. Por eso tampoco son confiables `MATRIZ_OD_NODOS_Casanare6.csv`
ni la frase "18.6 km – 157.4 km" de la lámina de Transporte, que se calcularon desde esos nodos. Aquí NO se usaron.

## 2. Elevación de las cabeceras (msnm)

| Municipio   | **SRTM 30 m (usado)** | ASTER 30 m | Open-Elevation | Wikipedia (es) | ¿Consistente? |
|-------------|------:|------:|------:|------:|---|
| Yopal       | **337** | 337 | 336 | 390 | DEM coherente entre sí. Wikipedia +53 m |
| Aguazul     | **283** | 279 | 280 | 290 | Sí |
| Tauramena   | **456** | 454 | 456 | 460 | Sí |
| Monterrey   | **443** | 445 | 442 | 481 | Aproximado (+38 m en Wikipedia) |
| Sabanalarga | **473** | 470 | 473 | 450 | Sí (±25 m) |
| Villanueva  | **289** | 283 | 288 | 420 | **No.** Wikipedia +131 m, que no es plausible para el llano |

Fuentes / API:
- SRTM 30 m: `https://api.opentopodata.org/v1/srtm30m?locations=lat,lon|...` (NASA SRTM GL1)
- ASTER 30 m: `https://api.opentopodata.org/v1/aster30m?locations=...` (ASTER GDEM v3)
- Open-Elevation: `https://api.open-elevation.com/api/v1/lookup?locations=...`
- Wikipedia: parámetro `altitud` de la ficha (`es.wikipedia.org/w/index.php?title=<Municipio>&action=raw`)

Criterio: se usa SRTM 30 m porque los tres DEM independientes coinciden en ±6 m. Las cifras de Wikipedia son promedios municipales o de ficha
y en algunos casos no coinciden (Villanueva y Yopal), por lo que solo se registran como referencia.

Nota ArcGIS/IGAC: no se usó una capa de curvas de nivel IGAC. El valor viene del DEM SRTM, que es la misma base con la que se generan curvas
de nivel a 1:100 000 en ArcGIS. Las líneas de cota 300–700 m del perfil son cotas del DEM, no curvas IGAC digitalizadas.

## 3. Recorrido y distancias viales (abscisado)

Ruta real calculada con OSRM (`https://router.project-osrm.org/route/v1/driving/...`, red vial OpenStreetMap) entre las coordenadas de la sección 1:

| Tramo | km (OSRM) | Abscisa acumulada (km) |
|---|---:|---:|
| Yopal → Aguazul | 26.9 | 27.0 |
| Aguazul → Tauramena | 39.7 | 66.7 |
| Tauramena → Monterrey | 30.4 | 97.2 |
| Monterrey → Sabanalarga | 42.5 | 139.8 |
| Sabanalarga → Villanueva | 43.4 | 183.3 |

Orden: el orden de listado "Yopal, Aguazul, Monterrey, Sabanalarga, Tauramena, Villanueva" obliga a devolverse por la vía
(OSRM: 290.7 km, con Sabanalarga→Tauramena 70.6 km y Tauramena→Villanueva 73.1 km). Por eso el perfil usa el orden de recorrido
real sin retrocesos: **YOP → AGZ → TAU → MTY → SBL → VNV (183.3 km)**. La planta del hero sigue esquemática con el orden original.

## 4. Perfil del terreno

- 88 muestras SRTM 30 m, cada ~2 km sobre la geometría vial de OSRM (`profile.json`).
- Mínimo 256 msnm (llano entre Yopal y Tauramena). **Máximo 748 msnm en km 84.6**: cruce del piedemonte entre Tauramena y Monterrey.
- Escala en el SVG: horizontal 1020 px = 183.3 km. Vertical 0.18 px/m (200 msnm en la base). **Exageración vertical ≈32×**.
