# Cupos educativos — Corredor Ruta 65, Casanare (6 municipios)
Fecha: 2026-09-28. Solo fuentes oficiales. Nada inventado.

## 1. Qué es un cupo y cómo se mide
- Cupo = plaza ofertada por un establecimiento para un grado/sede en el proceso de gestión de cobertura (proyección de cupos → solicitud → asignación → matrícula), registrado en SIMAT (Res. MEN 7797/2015, proceso de gestión de cobertura).
- El MEN NO publica en datos abiertos los cupos ofertados ni la capacidad instalada por sede. Lo publicado es MATRÍCULA (SIMAT) y TASAS de cobertura (neta/bruta) sobre población DANE 5-16.
- Cobertura bruta = matrícula total / población 5-16; cobertura neta = matrícula en edad teórica / población 5-16. Bruta >100% indica extraedad o estudiantes de otros municipios, no "cupos sobrantes".

## 2. Normas / estándares
| Norma | Contenido | Fuente |
|---|---|---|
| Decreto 3020/2002, art. 11 (compilado en Dec. 1075/2015) | Promedio mínimo de alumnos por docente en la entidad territorial: **32 urbana y 22 rural**. Parámetros: preescolar y primaria 1 docente/grupo; secundaria y media académica 1,36 docentes/grupo; media técnica 1,7. | https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=6405 ; https://www.mineducacion.gov.co/portal/decadas/214905:Relaciones-tecnicas-alumno-docente |
| NTC 4595 (MEN/ICONTEC, 2015; versión 2025 publicada en MEN) | Ambiente A (aula) básica/media: hasta **40 estudiantes**, **1,65 m²/estudiante** (≈66 m²/aula); +0,10 m²/est. por cada 10 estudiantes menos de 40. | https://www.mineducacion.gov.co/1759/articles-355996_archivo_pdf_norma_tecnica.pdf ; https://www.mineducacion.gov.co/1780/articles-355996_recurso_14.pdf |
| Res. 2565/2003 | Parámetros de atención a población con necesidades educativas especiales (derogada/sustituida por Dec. 1421/2017). No fija alumnos/aula general: no aplica. | — |
Nota: cifras NTC tomadas del resumen del buscador sobre los PDF del MEN; verificar número de tabla/página en el PDF antes de citar textualmente.

## 3. Datos oficiales por municipio (verificados 2026-09-28)
Dataset: MEN "Estadísticas en educación en preescolar, básica y media por municipio", datos.gov.co id **nudc-7mev** (https://www.datos.gov.co/resource/nudc-7mev.json). Año más reciente: 2024.

| Municipio | Pob. 5-16 (2024) | Tasa matric. 5-16 | Cob. neta | Cob. bruta | Matrícula sedes (proyecto, SISE/MEN x5ay-984n) | Matrícula − Pob.5-16 (proyecto) |
|---|---|---|---|---|---|---|
| Yopal | 36.954 | 97,82 | 97,77 | 106,78 | 40.339 | +3.385 |
| Aguazul | 8.111 | 89,85 | 89,82 | 97,51 | 8.456 | +345 |
| Monterrey | 3.776 | 84,14 | 84,08 | 92,37 | 3.721 | −55 |
| Sabanalarga | 744 | 75,00 | 75,00 | 79,30 | 1.100 | +356 |
| Tauramena | 5.726 | 92,63 | 92,61 | 99,97 | 8.513 | +2.787 |
| Villanueva | 8.112 | 95,82 | 95,81 | 102,42 | 7.685 | −427 |

Comparación con el proyecto: `Indicadores_Educacion_Municipio_Casanare6.csv` coincide exactamente con nudc-7mev 2024 (población, tasas). OJO: la columna `cupos_vs_matriculados_basica_media` NO son cupos: es matrícula por sedes − población 5-16 (ej. Yopal 40.339−36.954=3.385). Debe rotularse "matrícula vs. población en edad escolar", no "cupos". La matrícula por sedes incluye extraedad, adultos/ciclos y otros niveles, por eso supera la población en Sabanalarga y Tauramena.

## 4. Referente defendible
- Déficit/superávit de cobertura: usar **cobertura neta 2024 (nudc-7mev)**; brecha = 100 − cobertura neta (Sabanalarga 25 pts, Monterrey 16, Aguazul 10 son los críticos).
- Alumnos por aula: **40 estudiantes/aula y 1,65 m²/est. (NTC 4595)** como capacidad física máxima.
- Alumnos por docente: **32 urbano / 22 rural (Dec. 3020/2002 art. 11)**.
- Capacidad teórica en cupos = n.º aulas × 40 — requiere inventario de aulas (no disponible).

## 5. No verificable públicamente
- Cupos ofertados/disponibles por sede o municipio (SIMAT es de acceso restringido a secretarías).
- Número de aulas / capacidad instalada por sede (búsqueda documentada en `_REPORTE_T2_I05_CUPOS.txt`; DUE 4fr3-hhfy y 28t6-6wvz devolvieron 403).
- Planta docente por municipio en datos abiertos (no consultada con éxito); por tanto relación alumno/docente real no calculable.
- Secretarías de Educación de Casanare y Yopal: sin cifras de cupos publicadas localizadas; solicitarlas por derecho de petición.
