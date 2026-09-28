# Cambios de integración, fase 1 (solo correcciones) — 2026-09-28
Backup: `index.html.bak_antes_integracion`. Verificado en el navegador (puerto 8962, ya detenido): 34 gráficas dibujan sin errores y la consola queda limpia; a 375 px no hay scroll horizontal; viewport vuelto a desktop.

## Transversal
- Coma decimal (es-CO) en todas las tarjetas, ledes y hallazgos que se tocaron, y en las referencias asociadas; `1.189.1 ha` → `1.189,1 ha`. El contador (`parseNum`) ya admite la coma.
- Contraste: `.label` de lámina, `.ln-c span`, `.dim`/`.dim-txt`, `.margin-note`, `.hero-index span`, `.hero-coords` y `.hero-vert` pasan de #4A7FA7 a #1A3D63.
- Clases nuevas: `.chart-note` (nota al pie de gráfica) y `.glosario`, con glosario breve en PL.02 (cobertura neta), PL.03 (IPS, isócrona), PL.05 (coroplético, índice), PL.07 (TPD, V/C, nivel de servicio) y PL.08 (EPE, conservador, cobertura 300/500 m).

## PL.01
- 954 → **879** (lede, tarjeta, pie de mapa, c-g2 n=, referencias); Yopal 523 / 54.8 % → **462 / 52,6 %**; 52 % → **56,9 %** (dep. 252 + educ. 248).
- DATA: ABASTECIMIENTO [1,1,1,1,0,0] = 4; INSTITUCIONAL [5,2,1,1,1,1] = 11; SEGURIDAD Yopal 17 (28); TRANSPORTE [12,8,6,1,2,1] = 30; total 879.
- Hallazgo: «87 % abastecimiento / 73 % seguridad» → salud 95/130 (73 %, consolidado), rotulado frente a las 119 de 157 sedes IPS de PL.03; seguridad 17/28 (61 %); 1 de las 4 plazas reales.
- Leyendas: «Educacion» → «Educación».
- c-g3 rotulada «consolidado antes de depurar, n=954» (no hay reparto urbano/rural depurado).
- c-g4 rotulada como «básicos (4 categorías, n=482) … vs. media de los 6 municipios»; la línea pasa a «Media de los 6 municipios».

## PL.02
- 270 → **256** (186/70; 146 y 110 rurales/urbanas), c-e1/c-e4/c-e5 y referencias; matrícula 69.814/284/246 → **66.525/281/237**; matSede y la línea de referencia 280,7.
- Educación superior: «23 puntos» → **9** instituciones distintas (el dato viene de la instrucción del coordinador y no se recontó aquí).
- Etiquetas de cobertura neta y sedes por 1.000 niños; línea «Cobertura plena (100 %)».

## PL.03
- c-s4: se añade la columna 2021 vacía (null). 2022: UCI 43, quirófanos/cirugía 15, adultos 335 (rotulado «incluye obstetricia reclasificada»), obstetricia null. `spanGaps:false` y tooltip «sin dato». Nota al pie y referencia nueva s2ru-bqt6.
- c-s1: título y series con unidad y año; eje «Tasa (unidad en la leyenda)».
- Tarjetas: año de las camas, «la menor», modo de viaje; hallazgo UCI 17 → 26 → 43.

## PL.04 (errores #9 y #10 de la auditoría de datos)
- Tasa Yopal 10.39 → **10,00**; N=340 → **n=329**; DATA de bibliotecas, centros culturales, teatros, tasas y composición.
- Teatros 3 de 6 → **4 de 6** (se añade Aguazul, que solo tiene una sala de cine).
- 253 deportivos rotulado frente a los 252 de PL.01; c-o2 «por 10.000 hab. (pob. DANE 2018)».

## PL.05
- Verificado en los shp: alcaldías 9 → **6** (duplicado en Tauramena, SIN_NOMBRE en Aguazul, CASA FISCAL); policía 16 → **15** (Yopal 11 → 10, sin el «Parqueadero de Fiscalia»). Cambian la matriz, c-i2 (n=45 → 41) y un hallazgo nuevo.
- Tarjetas: «1.00» → «8 de 8» (índice 1,00); 0.38 → **0,375**, que se aplica a Aguazul y a Monterrey; 0,25. El eje de c-i4 explica la fórmula y el pie de mapa, el coroplético.

## PL.06
- Cada registro es una **víctima**: la serie suma 2.700 (628 + 2.072), así que lede, c-t1, c-t2, c-t3, las tarjetas y las referencias pasan a «víctimas». 2.753 → 2.700 (2016–2025, sin los 53 de 2026); −67 % rotulado como víctimas; «2 · 3» → «2 y 3»; 4,3; distancias aproximadas entre nodos DANE.

## PL.07
- c-v4: max 0.35, paso 0,05. Composición 60,8/2,7/36,5 = 100,0, con tooltip sin el porcentaje repetido.
- Tarjeta V/C: «V/C = volumen/capacidad, tramo 65130, estación INVÍAS 932, TPD 4.527; estimación propia, método sin fuente documentada». «80 · 1» → «80 y 1». −66 % «frente a enero de 2020». Títulos de c-v1 a c-v4.

## PL.08
- Tarjeta: «1.189,1 ha» de parques, con las rondas hídricas **aparte** (141,9 ha). En el CSV son funciones distintas, no un subconjunto, así que no se escribió «de ellas».
- Aguazul 652,9 ha: tres polígonos OSM «SIN NOMBRE» de 212,8, 101,4 y 75,9 ha (leisure=park/recreation_ground), sin nombre ni fuente oficial. **No se pudo confirmar** que sean o no parques urbanos, así que la cifra se mantiene con una nota en la tarjeta, en c-p1 y en las referencias. El EPE conservador (<5 ha) ya los excluye, así que las cifras clave (Aguazul 7,68, Yopal 3,29) no cambian.
- «4 de 6» rotulado como conservador (2 de 6 con el bruto); títulos de c-p1, c-p3 y c-p4.

## Dejado fuera
- Reparto urbano/rural depurado de c-g3 (hace falta recalcular en SIG).
- Opción completa de c-g4, con las tasas sobre los 879 limpios.
- Rótulo «Yopal – Paz de Ariporo» del tramo 65130 en los datos de c-v4: no se verificó a qué tramo corresponde de verdad; la tarjeta ya lo liga a la estación 932.
- Conversión a coma de decimales en láminas y textos no tocados (portada, algunas referencias y tooltips de Chart.js, que ya usan es-CO).
- Hallazgo 3 de la auditoría visual (enlaces 502/403): sin verificar a mano.
- Unificación INVIAS/INVÍAS y «hab.» en todo el documento; solo se aplicó en lo editado.
