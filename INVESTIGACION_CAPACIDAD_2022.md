# Investigacion: capacidad instalada Casanare 2022 (los "0")

## Fuente de la serie del proyecto
- `hacer_graficas.py` / `T3_I08_CAPACIDAD_CASANARE_2017_2022_DEPTO.csv` usan **fa2g-cdft** (datos.gov.co, MinSalud): "Cantidad de ambulancias, camas y salas (consideradas trazadoras) por departamento, año y naturaleza jurídica para servicios habilitados". Ultima actualizacion 2022-11-21. Años: 2017, 2018, 2019, 2020, 2022 (**no hay 2021 ni 2023**).
  https://www.datos.gov.co/resource/fa2g-cdft.json

## Hallazgo: los 0 de 2022 son un ARTEFACTO por cambio de definicion (Res. 3100/2019), no valores reales
1. En fa2g-cdft el colapso es **nacional**, no solo Casanare: UCI adulto 6.149 (2020) -> 1.001 (2022); obstetricia 7.207 -> 387; quirofanos 3.080 -> 392. Camas adultos sube 39.099 -> 45.262.
2. El dataset desagregado del mismo corte, **s2ru-bqt6** (corte REPS 5-nov-2022, https://www.datos.gov.co/resource/s2ru-bqt6.json), muestra que en 2022 REPS usa nombres nuevos (Res. 3100/2019): "Intensiva Adultos" (6.007 nal.) en vez de "Cuidado Intensivo Adulto" (1.001, solo prestadores en norma antigua); "Sala de Cirugía" (3.004) en vez de "Quirófano" (392); "Obstetricia" casi desaparece y las camas pasan a "Adultos"/"TPR"/"Atención del Parto". fa2g-cdft solo sumo las etiquetas antiguas -> 0 en Casanare.
3. Casanare en s2ru-bqt6 (2022): Adultos **335** (coincide con la serie), Intensiva Adultos **43** (4 registros), Intermedia Adultos 24, Sala de Cirugía **15** (8 sedes), TPR 60, Atención del Parto 2, Salas de partos 23, Pediátrica 71.

## Coherencia
- UCI adulto: 17 (2017-19) -> 26 (2020, COVID) -> 43 (2022): crecimiento plausible. La capa IPS_SEDES_6M (115 UCI, 2026, corte c36g-9fc2 mar-2026) refleja la expansion posterior; no contradice.
- Quirofanos: 15-16 (2017-20) -> 15 (2022): estable; coherente con 12 sedes con cirugia en 2026 en los 6 municipios.
- Camas adultos 335 ≈ 274 adultos + 67 obstetricia de 2020 (341): consistente con que obstetricia se absorbio en "Adultos" -> 2022 no es comparable directamente en ese rubro.

## Valores recomendados para reemplazar 2022
| Indicador | 2022 | Nota |
|---|---|---|
| camas_adultos | 335 | incluye camas obstetricas reclasificadas |
| uci_adulto | 43 | "Intensiva Adultos" s2ru-bqt6 |
| obstetricia | sin dato | categoria eliminada; alternativa: TPR 60 (no equivalente) |
| quirofanos | 15 | "Sala de Cirugía" s2ru-bqt6 |

2021 y 2023: **sin dato oficial** en datos.gov.co (busqueda catalogo Socrata "capacidad instalada camas" solo devuelve datasets municipales de Barranquilla y Bucaramanga). El portal REPS (reps.minsalud.gov.co) solo ofrece el corte vigente, no historicos.
