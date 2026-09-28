# Rural vs urbano: sedes y equipamientos, Corredor Ruta 65 (Casanare)

## (a) Datos reales

### Sedes educativas (fuente: `02_EQUIPAMIENTOS_EDUCATIVOS/Sedes_Educativas_NivelEdu_Casanare6.dbf`, campos ZONA, SECTOR, MATRICULA; origen DANE-SISE 2023 / MEN-DUE 2019)

| Municipio | Sedes rurales | Sedes urbanas | Oficiales rurales | Matrícula rural | Matrícula urbana | Mediana matrícula/sede rural | Mediana/sede urbana |
|---|---|---|---|---|---|---|---|
| Aguazul | **33** | 18 | 33 de 33 | 1.922 | 6.527 | 16 | 171,5 |
| Monterrey | **9** | 5 | 9 de 9 | 467 | 3.254 | 28 | 547 |
| Sabanalarga | **11** | 2 | 11 de 11 | 224 | 874 | 9 | 437 |
| Tauramena | **21** | 7 | 21 de 21 | 1.835 | 6.678 | 27 | 1.144 |
| Villanueva | 10 | 11 | 10 de 10 | 943 | 6.740 | 82 | 368 |
| Yopal | 69 | 74 | 63 de 69 | 8.567 | 31.759 | 16 | 131,5 |
| **Total** | **153** | **117** | | 13.958 | 55.832 | | |

Esta tabla cuadra con `GRAFICAS_Educacion_Casanare6_datos.csv` (5_sedes_por_zona: 153 rurales, 117 urbanas).

En 4 de los 6 municipios hay más sedes rurales que urbanas (Aguazul, Monterrey, Sabanalarga y Tauramena). Aun así, el sector rural tiene el **57 % de las sedes y solo el 20 % de la matrícula**. La sede rural típica atiende entre 9 y 28 estudiantes; la urbana, entre 130 y 1.100.

### Equipamientos en general (`01_EQUIPAMIENTOS_GENERAL/GRAFICAS_General_Casanare6_datos.csv`)
- En el inventario general, EDUCACION sale con menos rurales que en la capa de sedes: Tauramena 15R/9U, Sabanalarga 5R/3U, Monterrey 6R/6U, Aguazul 23R/25U, Villanueva 1R/15U y Yopal 61R/79U (248 registros frente a 270 sedes). **Las dos fuentes no coinciden.** Hay que explicar en la lámina cuál se usa.
- En las demás categorías predomina lo urbano (salud, deporte, cultura, institucional). La única excepción es TRANSPORTE, que es mayoritariamente rural en Aguazul (7/3), Villanueva (6/0) y Yopal (10/3).
- Conclusión: el "predominio rural" es un fenómeno **de las sedes educativas**, no de todos los equipamientos.

## (b) Causas

1. **Modelo Escuela Nueva: escuelas multigrado para población rural dispersa.** VERIFICADO. El MEN lo define como un modelo para escuelas rurales multigrado en zonas de alta dispersión poblacional, con 1 a 3 docentes para los cinco grados de primaria. Fuente: MEN, "Escuela Nueva", Modelos Educativos Flexibles, portal vigente: https://www.mineducacion.gov.co/portal/Preescolar-basica-y-media/Modelos-Educativos-Flexibles/340089:Escuela-Nueva
2. **Política de educación rural: se atiende con muchas sedes pequeñas cerca de las veredas.** VERIFICADO.
   - MEN, Plan Especial de Educación Rural (PEER), dic. 2020: https://www.mineducacion.gov.co/1780/articles-404773_Recurso_01.pdf
   - MEN, Proyecto de Educación Rural (PER): https://www.mineducacion.gov.co/portal/Preescolar-basica-y-media/Proyectos-Cobertura/329722:Proyecto-de-Educacion-Rural-PER
   - El PEER describe el modelo de nucleación: una sede central articulada con sedes seccionales (Concentraciones de Desarrollo Rural, 1973).
3. **Las sedes urbanas son pocas y grandes.** VERIFICADO con los datos del proyecto (tabla anterior). Además, el 100 % de las sedes rurales de 5 municipios son oficiales, mientras que el sector privado se concentra en lo urbano (Yopal tiene 52 sedes urbanas no oficiales). Por eso el número de sedes no mide capacidad: la matrícula urbana es 4 veces la rural.
4. **Integración de sedes en instituciones educativas (Ley 715 de 2001, art. 9; compilado en el Decreto 1075 de 2015).** INFERIDO. Esto no lo abrí en esta sesión. Según esta norma, una institución educativa agrupa varias sedes, así que cada escuela veredal cuenta como sede aunque dependa de una IE urbana o de un centro poblado. Debe verificarse en https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=4452 (Ley 715) y en el Decreto 1075/2015 antes de citarlo.
5. **Territorio rural extenso y población dispersa.** INFERIDO. Es coherente con Escuela Nueva (punto 1). Sin embargo, **no obtuve** las áreas urbana/rural ni la población por clase (cabecera / centros poblados y rural disperso, CNPV 2018) de estos municipios en DANE.

## (c) Párrafo para la lámina 02
> En Aguazul, Monterrey, Sabanalarga y Tauramena hay más sedes educativas rurales que urbanas (153 frente a 117 en el corredor). Sin embargo, esas sedes rurales, todas oficiales en 5 de los 6 municipios, reúnen solo el 20 % de la matrícula, con una mediana de 9 a 28 estudiantes por sede. Esto responde al modelo de escuelas rurales multigrado (Escuela Nueva, MEN), pensado para población dispersa: muchas sedes pequeñas en el campo y pocas sedes grandes en la cabecera. Por eso "más sedes" no significa "más capacidad".

## (d) Lo que no se verificó
- Áreas urbana y rural de cada municipio y población por cabecera y rural disperso (DANE CNPV 2018 o proyecciones). No se consultó.
- El texto exacto de la Ley 715/2001 art. 9 y del Decreto 1075/2015 sobre sedes. No se abrió; es inferido.
- Qué sedes rurales aplican efectivamente Escuela Nueva: el dbf no tiene campo de modelo. Se podría obtener del DANE EDUC (variable MODELOEDUC_NOMBRE): https://microdatos.dane.gov.co/index.php/catalog/834
- Los PDM/POT municipales no se consultaron.
- La discrepancia de conteo entre la capa de sedes (270) y el inventario general (248) no está resuelta. Además, algunas sedes vienen de MEN-DUE 2019 (fallback) y otras de 2023.
