# Auditoría lámina 05: Terminales, Militar y Fiscalías/Justicia (2026-09-28)

Script: `05_EQUIPAMIENTOS_INSTITUCIONALES\depurar_terminales_militar.py`. Los originales no se tocaron; los backups llevan el sufijo `.bak_antes_terminales`.
Salidas nuevas: `Terminales_Terrestres_Casanare6_dedup.shp`, `Aerodromos_Pistas_Casanare6.shp`, `Militar_Casanare6_instalaciones.shp`, `Fiscalias_Justicia_Casanare6_depurado.shp`, `T5_I01_MATRIZ_PRESENCIA_Casanare6_v2.csv`, `GRAFICAS_Institucional_Casanare6_datos_v2.csv`, `hacer_mapa_final_v2.py` y `MAPA_FINAL_Institucional_Casanare6_v2.png` (copiado a `mapas/Institucional.png`).

## Criterios
- **Terminales de transporte terrestre**: solo SUBCATEG=TERMINAL_TERRESTRE (amenity=bus_station). Los puntos a menos de 300 m entre sí y en el mismo municipio se cuentan como una sola instalación. Los aeródromos, pistas y helipuertos (aeroway=aerodrome, 27) salen de la matriz. Se conservan en `Aerodromos_Pistas_Casanare6.shp` y en la columna `Aerodromos_Pistas_fuera_matriz`.
- **Militar**: se cuenta la instalación, no cada geometría OSM. Los checkpoints, las oficinas sin nombre y las áreas de entrenamiento se integran al complejo al que pertenecen. Un checkpoint aislado no se cuenta.
- **Fiscalías/Justicia**: solo amenity=courthouse. Se descartan restaurantes, tiendas naturistas, colegios, un pozo petrolero, un mirador, hoteles, un paradero y un parqueadero. Dos sedes a menos de 300 m se cuentan como un solo complejo.

## Terminales: 33 registros → 3 instalaciones
| Municipio | Antes | Aeródromos/pistas (fuera) | Terminales terrestres |
|---|---|---|---|
| Yopal | 13 | 10 | 2 |
| Aguazul | 10 | 7 | 1 |
| Villanueva | 6 | 6 | 0 |
| Monterrey | 2 | 2 | 0 |
| Sabanalarga | 1 | 1 | 0 |
| Tauramena | 1 | 1 | 0 |

Decisiones:
- **Yopal**: se fusionan "Terminal de Transporte" (ID 28; -72.39068, 5.33525) y "Terminal Yopal" (ID 32; -72.39054, 5.33558), que están a unos 40 m. Se deja aparte "Nuevo Terminal de Transporte de Yopal" (ID 30; -72.42506, 5.31352), a unos 4,5 km: es otra instalación, en Llano Lindo. No se verificó si la terminal antigua sigue en operación.
- **Aguazul**: se fusionan en 1 los IDs 25 "Terminal Aguazul" (-72.54993, 5.16888), 29 SIN_NOMBRE (-72.54978, 5.16860) y 31 "Terminal de Transportes Aguazul" (-72.54990, 5.16872), que están a menos de 35 m.
- **Aeródromos excluidos (27)**:
  - Yopal: Balmoral, Bamoruco, Marapaca, Germania, Los Cabros, Trompillos, Betania Runway, El Zamuro (con fixme "Not Present"), El Araguaney y El Alcaraván.
  - Aguazul: El Moriche, El Porvenir, El Recreo, Fasca Main Base, Jamaica, La Gloria y Maríaangélica.
  - Villanueva: Santa Barbara, Santa Helena, Colegial, Santa Helena de Upía, Soceagro y Villanueva (SKVN).
  - Monterrey: La Florida y Las Tinieblas.
  - Sabanalarga: Aguaclara.
  - Tauramena: La Envidia.

## Militar: 29 geometrías → 4 instalaciones
| Instalación | Municipio | IDs de la capa fusionados |
|---|---|---|
| Brigada XVI (incluye Gaula Casanare, los controles Entradas/Salidas/Control del CRM y 8 oficinas) | Yopal | 1,2,3,18-27 (13) |
| Fuerza Aérea, área militar del aeropuerto El Alcaraván (landuse=military, 5 oficinas y 1 training_area) | Yopal | 4,8-13 (7) |
| Base de Telecomunicaciones EYP (3 geometrías "Telecommunications base", 1 base con nombre y 2 bases sin nombre) | Yopal | 6,7,14-17 (6) |
| Batallón No 44 Ramón Nonato Pérez | Tauramena | 5 |
| **Excluidos**: ID 28 "P.C - Comando Yopal" (military=checkpoint) e ID 29 (oficina sin nombre junto al 28) | Yopal | 28, 29 |

Yopal pasa de 28 a 3 y Tauramena se mantiene en 1. La categoría sigue presente en los mismos municipios.

## Fiscalías/Justicia: 34 → 3 (todas en Yopal)
Se conservan: ID 5 "Palacio de justicia" + ID 32 "Juzgado Departamental" (a unos 110 m, se cuentan como un complejo), ID 33 "URI" e ID 34 "Antiguo palacio municipal" (courthouse).
Se descartan 30 registros. Ejemplos:
- Restaurantes: Mi Llanurita, Yurimar, Canaima.
- 15 tiendas o centros naturistas.
- Colegios: Inst. La Sabiduría, La Upamena.
- Laguna Jurijuri (mirador), el pozo Curiara 1, Juriscoop (oficina financiera) y Seguridad Estelar (tienda).
- Hotel y fincas turísticas, un paradero de colectivos y el "Parqueadero de Fiscalia".

Como consecuencia, Aguazul (3 → 0), Monterrey (5 → 0), Tauramena (1 → 0) y Villanueva (1 → 0) pierden la categoría.

## Matriz antes → después
| Municipio | Fisc. | Militar | Term. | N cat. | Índice |
|---|---|---|---|---|---|
| Yopal | 24→3 | 28→3 | 13→2 | 8→8 | 1.000→1.000 |
| Villanueva | 1→0 | 0 | 6→0 | 6→4 | 0.750→0.500 |
| Tauramena | 1→0 | 1 | 1→0 | 6→4 | 0.750→0.500 |
| Monterrey | 5→0 | 0 | 2→0 | 5→3 | 0.625→0.375 |
| Aguazul | 3→0 | 0 | 10→1 | 4→3 | 0.500→0.375 |
| Sabanalarga | 0 | 0 | 1→0 | 3→2 | 0.375→0.250 |

El total del corredor en la gráfica c-i2 pasa de n=131 a n=45.

## Cambios en index.html
- Lámina 05: lede, pie de foto del mapa, cifras de Aguazul (0.38) y Sabanalarga (0.25), hallazgos, DATA.institucional (mat, tot, idx, ncat, rótulo "Terminales terrestres") y títulos n=45.
- Lámina 06: la ficha "Aeródromos Aerocivil · terminales" pasa de 2 · 33 a 2 · 3.
- Referencias: filas de terminales, militar, justicia e índice.
- Mapa regenerado: Tauramena y Villanueva pasan de la clase "6 categorías" a "4-5"; Aguazul y Monterrey quedan en "1-3".

## Sin verificar
- La operación actual de la terminal antigua de Yopal.
- Si Gaula ocupa una sede propia.
- Policía 11 en Yopal, que puede estar inflado igual que las demás categorías y no se auditó.
- Subregistro OSM de juzgados y fiscalías fuera de Yopal: su ausencia en el mapa no significa que no existan.
- El PNG de matplotlib `GRAFICAS_Institucional_Casanare6.png` no se regeneró (index.html usa Chart.js).
