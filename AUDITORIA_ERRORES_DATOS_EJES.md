# Auditoría de gráficas Chart.js — errores de escala/ejes/datos

Archivo auditado (solo lectura): `repo_diagnostico\index.html` (bloque `<script>` final, líneas ~1230-1330, funciones `bar()`, `mk()`, `ds()`, `axes()`, `refLine()`).
Fuente de contraste: CSVs en `INFO_GIS\BASE_RICARDO\ENTREGA_25_SEP_2026\01_...` a `08_...`.

Se revisaron las 33 gráficas de las 8 láminas (General c-g1..g4, Educación c-e1..e6, Salud c-s1..s4, Social c-o1..o4, Institucional c-i1..i4, Transporte c-t1..t4, Vías c-v1..v4, Espacio Público c-p1..p4) y los `stat-val` de cada lámina contra sus gráficas/CSV.

## 1. ERROR CONFIRMADO — Lámina 02 Educación, gráfica G-02 "Cobertura neta educativa por municipio" (canvas `c-e2`, línea 1275 del script)

- **Config actual:** `bar('c-e2', e.mun, [ds('Cobertura neta', e.cob, C.a), refLine('100 %',100,...)], 'Cobertura neta (%)', {suf:' %', legend:true, y:{max:110}})`
- **Dato fuente** (`GRAFICAS_Educacion_Casanare6_datos.csv`, bloque `2_cobertura_neta_municipio`): Yopal 97.77, Aguazul 89.82, Monterrey 84.08, Sabanalarga 75.00, Tauramena 92.61, Villanueva 95.81. Máximo real de los datos: **97.77 %**.
- **Diagnóstico:** el dato fuente es correcto; el problema es puramente de configuración del eje Y. `max:110` es un valor "feo" sin justificación lógica — la cobertura neta es un porcentaje que no puede superar el 100 % de forma sustantiva (la línea de referencia `refLine('100 %',100,...)` ya marca el techo teórico), así que dejar 10 puntos de aire por encima del 100 % genera la lectura visual de que hay municipios superando el 100%, cuando en realidad el máximo es 97.77 %.
- **Corrección sugerida:** cambiar `y:{max:110}` por `y:{max:100}` (igual que ya se hizo correctamente en `c-p3`, línea 1323, `y:{max:100}` para el mismo tipo de dato — % de área cubierta). Esto alinea la gráfica de Educación con el patrón ya usado en Espacio Público para variables de porcentaje con techo teórico en 100.

## 2. Inconsistencia de rango de años — Lámina 06 Transporte, gráfica G-06 "Siniestros por gravedad" (canvas `c-t3`, línea 1008 HTML)

- **Título mostrado:** "Siniestros por gravedad. n=2.753 (ANSV **2016-2026**)"
- **Dato fuente** (`GRAFICAS_Transporte_Casanare6_datos.csv`, bloque `linea_tiempo_anio`): la serie real va de **2016 a 2025** (368, 395, 334, 419, 236, 193, 285, 173, 157, 140 — 10 años, ningún dato de 2026). El resto de la lámina (lede y gráfica `c-t1` "Siniestros viales por año (2016-2025)") usa correctamente 2016-2025.
- **Diagnóstico:** es un error cosmético de etiqueta (probablemente arrastrado al copiar el texto de otra lámina que sí llega a 2026, como Educación con proyecciones DANE 2018-2026). El dato agregado n=2.753 sí es correcto (637+2.116 = 2.753, y también coincide con la suma de la serie anual), solo el rango de años en el título está mal.
- **Corrección sugerida:** cambiar el `<h3 class="chart-title">` de `c-t3` (línea 1008) de "ANSV 2016-2026" a "ANSV 2016-2025" para que coincida con el resto de la lámina y con los datos reales.

## 3. Revisado y SIN errores — resto de configuraciones de ejes con `max`/`min`/`suggestedMax`

- `c-i4` (Institucional, índice de presencia 0-1): `min:0, max:1` — correcto, el índice está definido en [0,1] por construcción (n categorías presentes / 8).
- `c-v4` (Vías, relación V/C): `y:{max: Math.max(...)*1.6}` — es un cálculo dinámico de margen visual (no un número "feo" fijo tipo 110/347), práctica aceptable para dar aire sobre la barra más alta; no es un bug.
- `c-p1` (Espacio Público, EPE bruto/conservador): eje logarítmico `min:1`, con línea de referencia en 15 m²/hab (estándar OMS/ONU-Hábitat) — coherente con los datos (rango de EPE muy disperso, de <1 a >100 m²/hab según municipio), el log-scale está bien justificado.
- `c-p3` (Espacio Público, accesibilidad 300/500 m): `y:{max:100}` — correcto, ya usa el patrón adecuado para %.
- `c-s2` (Salud, % área a ≤30 min IPS): sin `max` fijo, autoescalado — correcto, porque el dato real es bajo (máx. 11.44 %) y forzar un eje 0-100% aplanaría la gráfica; el autoescalado aquí es la decisión correcta, no un bug.
- Resto de gráficas de barras/líneas (`c-g1..g4`, `c-e1`, `c-e3..e6`, `c-s1`, `c-s3..s4`, `c-o1..o4`, `c-i1..i3`, `c-t1..t2`, `c-t4`, `c-v1..v3`, `c-p2`, `c-p4`) usan la función genérica `axes()` sin límites fijos "feos" — autoescalado estándar de Chart.js, sin señales de bug.

## 4. Consistencia tile vs. gráfica (cifras destacadas `stat-val`)

Se verificaron los `stat-val` de las 8 láminas contra sus CSV/gráficas:
- Educación: `75.00 %` (mínimo de cobertura neta, Sabanalarga) — coincide exactamente con el CSV (75.0).
- Salud: `1.008` camas Yopal (de 1.153 del corredor) — coincide con la suma de `camas_totales` del CSV (1008+58+58+4+10+15=1153); `157` sedes IPS coincide con la suma de `n_sedes_total`; `3.33 %` Tauramena coincide con `pct_area_30min_cualquier_ips`.
- Transporte: `−67 %` (419→140) = -66.6 %, redondeo aceptable a -67 %; `2.753` siniestros coincide con 637+2.116 y con la suma de la serie anual 2016-2025.
- No se encontraron discrepancias numéricas entre tiles y sus gráficas/CSV correspondientes en las 8 láminas.

## 5. Etiquetas de eje / unidades

No se detectaron unidades mal etiquetadas (ej. "%" mostrando conteos enteros, o viceversa) en las configuraciones revisadas. Los sufijos (`suf:' %'`, `' m²/hab'`, `' km'`, `' siniestros'`, `' veh.'`) corresponden a los datos que alimentan cada `ds()`.

## 6. Pendiente / no verificado en esta pasada

- No se sirvió el archivo con `python -m http.server` ni se tomaron screenshots de las 8 láminas para revisar recorte/superposición visual de etiquetas de eje (punto 5 del encargo), porque el archivo está siendo editado en paralelo por otro agente y se priorizó el análisis estático de configuración + contraste con CSV para no interferir. Se recomienda una pasada visual adicional una vez el otro agente termine sus ediciones, específicamente en `c-i3` (matriz de presencia/ausencia, que no usa Chart.js sino `chart-matrix` custom) y en `c-t4`/`c-v3` (ejes Y con muchas etiquetas de municipios que podrían solaparse).

## Resumen de acciones sugeridas (prioridad)

1. **Alta** — `c-e2` (línea 1275): cambiar `y:{max:110}` → `y:{max:100}`.
2. **Baja (cosmético)** — `c-t3` título (línea 1008 HTML): cambiar "ANSV 2016-2026" → "ANSV 2016-2025".
3. Sin cambios necesarios en el resto de las 31 gráficas revisadas.
