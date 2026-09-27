# Auditoría visual — index.html (Diagnóstico Corredor Ruta 65)

Metodología: servido con `python -m http.server 8799`, recorrido completo con scroll y
screenshots reales en desktop (~800–1186 px) y móvil emulado (375 px), zoom en zonas
sospechosas, e inspección del árbol de accesibilidad (`read_page`) para ubicar elementos.
Auditoría de solo lectura — no se modificó `index.html`.

---

## 1. CRÍTICO — Huecos en blanco enormes entre secciones (desktop y móvil)

**Ubicación:** reproducido en varios puntos: entre el bloque "Planta esquemática + perfil
longitudinal" (portada) y el índice de láminas; entre "Fin de lámina · PL.05" y el inicio
de la Lámina 06 (Transporte); y entre "Fin de lámina · PL.06" y la Lámina 07. También
reproducido en móvil (375 px) bajando desde la Lámina 00 (Localización) hacia la Lámina 01.

**Descripción:** al hacer scroll aparecen tramos de **~700–1000 px completamente en
blanco** (sin texto, sin mapa, sin gráfica), a veces con solo una pequeña marca de cota
o línea decorativa aislada en el medio. El usuario tiene que seguir scrolleando "a
ciegas" para volver a encontrar contenido. Esto ocurre tanto si se hace scroll rápido
(varios "wheel ticks" seguidos) como al volver a pasar por la misma zona más despacio, por
lo que no parece ser solo un efecto de scroll-fling del navegador sino un problema real
de layout/alto reservado.

**Hipótesis técnica:** probablemente un contenedor (mapa, gráfica Chart.js o bloque de
"cifra clave") tiene una altura mínima/aspect-ratio reservada por CSS que no coincide con
el alto real de su contenido, o hay un elemento con `margin`/`padding` vertical
desproporcionado entre `section.lamina` y la siguiente. También es compatible con un
`IntersectionObserver` de "reveal on scroll" que reserva el alto final del bloque antes de
activar la animación, y que en ciertos casos no se dispara.

**Sugerencia de corrección:** revisar cada `section` de lámina y quitar cualquier
`min-height`/`padding-block` fijo que no dependa del contenido real; si el efecto de
aparición usa `opacity`/`transform` con alto reservado, cambiar a que el alto se calcule
tras el render (`height:auto` con `transition` solo de opacidad/transform, no de layout).
Verificar también que no haya un elemento `position:absolute`/`fixed` invisible que esté
empujando el flujo del documento (agrega alto sin pintar nada).

---

## 2. MEDIO — Desbordamiento/margen muerto en el hero (portada) en móvil

**Ubicación:** Portada, viewport móvil 375 px.

**Descripción:** el bloque de portada (logos, "CORREDOR RUTA 65", texto de diagnóstico,
índice de láminas) no ocupa el ancho completo del viewport: queda pegado a la izquierda y
deja una franja vertical en blanco de **~90–100 px (≈20 % del ancho)** a la derecha,
como si el contenedor tuviera un `width` o `max-width` fijo en px en vez de `100%`/`vw`.
El resto de las láminas (Loc., L-01, L-02, L-03…) sí ocupan el ancho completo — el
problema es específico del `<header>`/hero de portada.

**Sugerencia de corrección:** revisar el selector del contenedor de portada (probablemente
`.hero`, `.portada` o similar) y sus reglas dentro del media query de móvil; cambiar
`width: <valor fijo>` por `width: 100%` / `max-width: 100vw` y confirmar que no tenga un
`grid-template-columns` heredado del layout desktop (título+norte+escala a la izquierda)
que no colapsa a una sola columna por debajo de cierto breakpoint.

---

## 3. MEDIO — Línea de cota/ornamento cruza el mapa L-03 en móvil

**Ubicación:** Lámina 00 · Localización, tarjeta "Escala corredor" (L-03), móvil 375 px.

**Descripción:** una línea horizontal gris fina (aparenta ser una marca de cota /
ornamento de margen) atraviesa la esquina superior del mapa "Corredor Ruta 65" justo
sobre las etiquetas de municipios, quedando por encima del contenido cartográfico en vez
de detrás o fuera de la caja del mapa.

**Sugerencia de corrección:** bajar el `z-index` del elemento decorativo (línea de cota)
por debajo del contenedor `.mapa`/`.card` en el breakpoint móvil, o excluirlo con
`display:none` en móvil si es puramente ornamental de escritorio.

---

## 4. MEDIO — Bajo contraste en leyendas de gráficas Chart.js

**Ubicación:** Lámina 03 · Salud, "Gráfica · Sedes y camas" (G-03), desktop. Patrón
probablemente repetido en G-01, G-02, G-04 … G-08 (misma paleta).

**Descripción:** las barras y el texto de leyenda ("Sedes IPS / 10.000 hab", "Camas /
1.000 hab") usan un azul medio (~`#4A7FA7`/`#B3CFE5`) sobre fondo azul muy claro
(`#F6FAFD`/`#B3CFE5`), lo que reduce mucho el contraste — las barras de menor valor casi
desaparecen contra el fondo de la tarjeta, y el texto de leyenda/ejes es difícil de leer a
tamaño de lámina impresa.

**Sugerencia de corrección:** para texto de ejes/leyenda usar el tono más oscuro de la
paleta (`#0A1931` o `#1A3D63`) en vez de `#4A7FA7`/`#B3CFE5`; reservar los tonos claros
solo para relleno de barras de fondo/grid, nunca para texto. Verificar contraste mínimo
4.5:1 con una herramienta de accesibilidad.

---

## 5. LEVE — El indicador de lámina activa en el nav sticky se desincroniza

**Ubicación:** nav superior sticky ("CORREDOR RUTA 65 … Loc 01 02 … PL.xx/08"), desktop,
reproducido varias veces al hacer scroll continuo.

**Descripción:** el número resaltado en el nav (p. ej. "05") y el badge "PL. 05 / 08" no
siempre coinciden con la lámina realmente visible en pantalla (se vio el separador "Fin de
lámina · PL.06" con el nav todavía marcando "05"). El desfase es de aproximadamente una
lámina completa durante scroll rápido.

**Sugerencia de corrección:** revisar el umbral (`rootMargin`/`threshold`) del
`IntersectionObserver` (o el cálculo de `scrollY` vs. `offsetTop`) que actualiza el nav
activo; ajustar el punto de disparo para que corresponda al tercio superior del viewport
en vez del centro/final de la sección anterior.

---

## 6. LEVE — Fila "Cifra clave" con métricas heterogéneas sin separación visual clara

**Ubicación:** Lámina 01 · Equipamientos General, panel lateral "Cifra clave", desktop.

**Descripción:** el texto "Sabanalarga, con 1 punto de salud" seguido inmediatamente del
número "30" en la misma fila da la impresión de una sola cifra mal formada ("...salud 30")
cuando en realidad son dos datos distintos apilados sin suficiente separación/etiqueta.
No es una superposición real, pero es fácil de leer mal a primera vista.

**Sugerencia de corrección:** aumentar el espacio vertical entre pares
label/valor dentro de `.cifra-clave` o anteponer una etiqueta corta a cada número para que
no parezcan continuación del texto anterior.

---

## Puntos verificados SIN problema (para que quede constancia)

- Cajetín (title block) con "RICARDO MARÍN", "CRISTHIAN GARCÍA", "FACULTAD DE
  INGENIERÍAS Y ARQUITECTURA": el texto se ve completo, sin truncar ni desbordar su caja,
  tanto en desktop como en los anchos probados.
- El mapa de Localización (L-01/L-02/L-03) y los mapas de las láminas 01 y 03 sí renderizan
  correctamente con su leyenda y símbolos — no hay superposición del cajetín ni de la
  numeración de lámina sobre el mapa en los casos revisados.
- No se detectó scroll horizontal no deseado de la página completa en desktop (sí hay una
  barra horizontal visible en algún estado intermedio de móvil, ver hallazgo 2, pero
  asociada al hueco del hero, no a overflow de una tarjeta individual).
- El texto vertical de margen ("PROYECTO 01 · CORREDOR RUTA 65 · PL.0X …") no se cruza con
  el contenido principal en los tramos revisados.

---

## Resumen priorizado

1. **[CRÍTICO]** Huecos en blanco masivos entre secciones — rompe la lectura continua del
   documento, en desktop y móvil.
2. **[MEDIO]** Hero de portada no ocupa el ancho completo en móvil (franja muerta a la
   derecha).
3. **[MEDIO]** Línea de cota decorativa se monta sobre el mapa L-03 en móvil.
4. **[MEDIO]** Contraste insuficiente en leyendas/barras de gráficas Chart.js.
5. **[LEVE]** Indicador de lámina activa en el nav se desincroniza durante scroll rápido.
6. **[LEVE]** Par de cifras clave sin separación visual suficiente en Lámina 01.
