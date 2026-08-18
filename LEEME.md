# Niseko Village — Onboarding Book

Libro digital de bienvenida para el equipo de invierno. 38 páginas, 10 idiomas.

**Todo está dentro de `index.html`**: la estructura, los estilos, la lógica y
los textos de los 10 idiomas. Solo las imágenes van aparte, en `img/`.

No hay que compilar nada. No hay npm, ni build, ni dependencias.

```
niseko-onboarding/
├── index.html      ← TODO está aquí (161 KB)
├── favicon.ico
└── img/            ← las fotos y los mapas
```

---

## Cómo probarlo

Haz **doble clic en `index.html`**. Se abre en el navegador y funciona.

(Al no haber archivos de código externos, ya no hace falta servidor local. Si
aun así quieres uno: `python3 -m http.server 8000`.)

---

## Cómo moverse por el archivo

`index.html` tiene unas 3.200 líneas, así que no se navega leyendo: se navega
buscando. Abre el archivo y usa **Ctrl+F** (Cmd+F en Mac) con estas marcas:

| Busca esto | Para cambiar |
|---|---|
| `SECCION: COLORES` | colores y tipografías |
| `SECCION: ESTILOS` | todo el CSS |
| `SECCION: DENSIDAD` | si un texto genera scroll |
| `SECCION: ESTRUCTURA` | añadir, quitar o mover páginas |
| `SECCION: AJUSTES` | velocidad, umbral de escritorio, idioma inicial |
| `IDIOMA: ESPANOL` | los textos en español |
| `IDIOMA: JAPONES` | los textos en japonés |
| `SECCION: MOTOR` | la lógica (normalmente no se toca) |

Los diez idiomas: `IDIOMA: ENGLISH`, `JAPONES`, `ESPANOL`, `TAILANDES`,
`TURCO`, `COREANO`, `CHINO`, `NEPALI`, `BIRMANO`, `VIETNAMITA`.

Esta misma lista está también al principio del propio `index.html`, por si
tienes el archivo delante y no este documento.

---

## Cómo editar un texto

Busca el idioma (por ejemplo `IDIOMA: ESPANOL`), baja hasta `pages`, y busca el
número de página. Cambia lo que hay entre las etiquetas:

```js
4: `
  <p>Hace mucho tiempo, Niseko era una región agrícola...</p>
`,
```

Cada idioma tiene cuatro partes:

```js
CONTENT.es = {
  ui:     { ... },   // botones y menús: "Empezar a leer", "Contenidos"
  labels: { ... },   // títulos de capítulos y secciones
  toc:    { ... },   // el índice
  pages:  { ... },   // el texto de cada página
};
```

### Etiquetas que puedes usar

| Etiqueta | Para qué sirve |
|---|---|
| `<p>...</p>` | Un párrafo |
| `<em>...</em>` | Cursiva |
| `<strong>...</strong>` | Negrita |
| `<br>` | Salto de línea |
| `<ul><li>...</li></ul>` | Lista de puntos |
| `<div class="subsec">...</div>` | Subtítulo azul |
| `<div class="alert-box">...</div>` | Aviso en rojo |
| `<div class="hotel-block"><div class="hotel-name">Nombre</div><p>...</p></div>` | Bloque de hotel |
| `<a class="info-link" href="...">...</a>` | Enlace pequeño |

### Reglas al editar

- Los textos van entre **acentos graves** `` ` ``, no entre comillas. Así puedes
  usar `"` y `'` dentro del texto sin problema.
- Si necesitas un acento grave literal, escríbelo `` \` ``.
- No cambies los números de página ni quites las comas del final.
- **Nunca escribas `</script>` dentro de un texto.** Cortaría el archivo por la
  mitad. Es lo único verdaderamente peligroso de tener todo junto.

### Si el libro deja de cargar

Casi siempre es una comilla o una coma. Abre la consola del navegador con **F12**,
pestaña *Console*: te dice el número de línea exacto.

Consejo: guarda una copia del archivo antes de una tanda grande de cambios. Con
todo en un solo archivo, un error de sintaxis deja el libro en blanco entero, no
solo un idioma.

---

## Cómo cambiar imágenes

Están en `img/` con nombres claros: `p1.jpg`, `p3.jpg`, `p5.jpg`… El número
coincide con el de la página.

**Para reemplazar una foto:** guarda la nueva con el mismo nombre encima de la
vieja. No hay que tocar código.

Formato recomendado: JPG vertical, alrededor de 960×1315 px, por debajo de
200 KB. Cuanto más pesan, más tarda en cargar en el móvil del staff.

Dos imágenes son especiales:

- `p23.jpg` es el mapa apaisado que ocupa las dos páginas en escritorio.
- `p23-mobile.jpg` es la misma imagen **girada físicamente** para que se lea en
  vertical en el móvil. Si cambias una, cambia la otra.

---

## Cómo añadir, quitar o mover páginas

Busca `SECCION: ESTRUCTURA`. Cada línea es una página:

```js
{ n: 5, type: 'img',  src: 'p5.jpg' },
{ n: 6, type: 'text', section: 'ourTeam' },
```

| `type` | Qué es | Necesita |
|---|---|---|
| `img` | Imagen a página completa | `src` |
| `text` | Página de texto | el texto en cada idioma |
| `text-img` | Texto arriba, imagen abajo | `src` + el texto |
| `split` | Una imagen partida en dos páginas | `src`, `half: 'left'` o `'right'` |
| `toc` | El índice (se genera solo) | — |
| `closing` | La página final de despedida | — |

Opciones extra:

| Opción | Efecto |
|---|---|
| `cover: true` | La imagen llena la página aunque tenga que recortar |
| `scroll: true` | Imagen larga con scroll vertical (horarios, mapas) |
| `srcMobile: 'x.jpg'` | Usa otra imagen en móvil y tablet |
| `desktopOnly: true` | La página solo sale en el spread de escritorio |
| `imgStyle: { maxWidth: '90%' }` | Estilos extra para la imagen |

Los títulos (`eyebrow`, `title`, `section`, `subsection`) apuntan a claves de
`labels`, para que se traduzcan solos.

**Si añades una página de texto**, añade su texto en los 10 idiomas. Si falta en
alguno, el libro muestra el inglés en vez de romperse. Acuérdate también de
actualizar los números del índice (`toc.items`) en los 10 idiomas.

---

## Cómo cambiar el comportamiento

Busca `SECCION: AJUSTES`:

```js
var CONFIG = {
  desktopMinWidth: 1024,   // a partir de aquí, dos páginas
  pageRatio:       0.72,   // proporción de una página
  mobilePageRatio: 0.58,   // en móvil la página es más alargada
  slideMs:         420,    // velocidad del pase de página
  swipeThreshold:  45,     // cuánto hay que deslizar el dedo
  defaultLang:     'en',   // idioma inicial
};
```

Si cambias `slideMs`, cambia también `--slide-speed` en `SECCION: COLORES`.

---

## Cómo cambiar los colores y la tipografía

Busca `SECCION: COLORES`:

```css
:root {
  --paper:   #f8f5ef;   /* fondo de las páginas */
  --ink:     #1a1a1a;   /* color del texto      */
  --accent:  #1a3a5c;   /* azul Niseko          */
  --accent2: #c8a96e;   /* dorado               */
  --night:   #0d1b2a;   /* fondo del lector     */
  --red:     #b03a2b;   /* avisos               */

  --font-display: "Fraunces", Georgia, "Times New Roman", serif;  /* titulos */
  --font-body:    "Lato", ...;                                    /* texto  */
}
```

`--font-display` es la fuente de los títulos: la portada, "Chapter 1", el
título de cada página, "Contenidos" y la página final. Cámbiala aquí y se
actualiza en todo el libro a la vez.

Las fuentes se cargan desde Google Fonts en el `<head>`. Si el sitio tiene que
funcionar sin internet, borra esas líneas: el libro usará Georgia y la
tipografía del sistema, y sigue viéndose bien (solo pierde el toque distintivo
de Fraunces).

### Los enlaces y los nombres de comercios

`.info-link` (los enlaces, como el de la web del ayuntamiento o las tiendas)
está a 18px.

Los tres nombres de comercios de la página del hanko (Hanko Hiroba, Cocoroya,
Ki Gift Shop) usan su propia clase, `.shop-name`, a 20px — separada de
`.subsec` a propósito, para no agrandar también otros subtítulos del libro
como "Get here by Train" o "Team Members Shuttle Bus", que siguen en 14px.

Si añades un nuevo nombre de comercio en cualquier idioma, usa
`<div class="shop-name">...</div>` (no `class="subsec"`) para que herede este
mismo tamaño.

### El selector de idioma de la portada

Es un `<select>` normal del navegador (busca `#langSelect` en el HTML y
`.lang-select` en el CSS) — no una cuadrícula de botones. El desplegable, el
resaltado de la opción elegida y el scroll cuando los 10 idiomas no caben en
pantalla los pone el propio navegador; no hay nada que programar ni ajustar
para eso.

La caja cerrada es oscura y translúcida (`rgba(255,255,255,.09)` sobre el
fondo de la portada), con texto blanco y la flechita en el dorado del sitio
(`--accent2`) — a juego con el resto del libro. Tiene una altura fija
(`height: 54px`) para que no cambie de tamaño al elegir un idioma con texto
más largo o más corto (birmano y vietnamita, por ejemplo).

Lo único personalizado es el aspecto de la caja cerrada: se le quitó la
flecha nativa (`appearance: none`) y se dibujó una a mano con
`.lang-select-wrap::after`. Si cambias el color o el tamaño de la caja,
recuerda que esa flecha es un elemento aparte, no forma parte del `<select>`.

Nota técnica: el fondo decorativo (`.welcome__aurora`) usa `position: fixed`
en vez de `absolute` a propósito: si vuelve a `absolute`, crea un scroll
"fantasma" en toda la portada aunque nada se vea cortado (es un bug conocido
de CSS al combinar centrado con overflow).

### La palabra "Chapter"

`.ch-eyebrow` controla la línea "Chapter 1" (o su traducción): va en azul
(el mismo `--accent` de los títulos), en cursiva, con la misma fuente de los
títulos. La primera letra no tiene un formato distinto — toda la palabra usa
exactamente el mismo estilo, tamaño y color. En japonés, chino, coreano,
tailandés, nepalí y birmano se queda recta (sin cursiva), porque esos
alfabetos no tienen forma cursiva propia y el navegador la simula inclinando
el trazo, lo cual queda torcido en vez de elegante — mira la regla con
`:lang(...)` justo debajo si quieres afinar cuáles idiomas se comportan así.

### Sin scroll en las páginas de texto

Las páginas de texto **no deben generar scroll**. Si añades texto y una página se
pasa de largo, no partas la página: busca `SECCION: DENSIDAD` y aprieta el
`line-height`.

Están comprobados los 10 idiomas en escritorio, portátil, tablet y móvil sin
scroll en ninguna página. Esto incluye el índice (página 2): sus entradas ya
no tienen un tamaño de letra fijo y pequeño, ahora crecen igual que el resto
del libro.

### El tamaño de letra es el mismo en todas las páginas — excepto la 2

El tamaño de los párrafos (`SECCION: DENSIDAD`) es uniforme: el mismo en todas
las páginas de texto, en todos los idiomas. Ronda los 24px en pantallas
grandes.

La página 2 (el índice) y el menú lateral "Contents" quedan **fuera** de este
tamaño a propósito: usan su propia variable, `--toc-font-size` (en
`SECCION: COLORES`), y no se mueven aunque subas o bajes la letra del resto
del libro. Los dos comparten el mismo valor entre sí, para que se vean
iguales.

`SECCION: AUTOAJUSTE` es la red de seguridad: si una página concreta queda
demasiado larga para su hueco, se encoge ella sola lo mínimo necesario para no
generar scroll. Con un tamaño base tan grande, las dos páginas más largas del
libro (la de los hoteles y la del horario de tren) necesitan encogerse bastante
en los móviles más pequeños — en el más estrecho probado llegan a verse más
pequeñas que el texto del índice. Es un compromiso inevitable: pediste 24px en
todas partes sin permitir scroll en ninguna, y esas dos páginas tienen mucho
más contenido que las demás. Si prefieres que no bajen tanto, la solución no es
tocar el CSS sino acortar el texto de esas dos páginas.

**Nota sobre las fuentes.** Fraunces y Lato tardan un poco en llegar de
Google Fonts. Hasta que cargan, el navegador mide el texto con la fuente de
reserva (Georgia o la del sistema), que no ocupa exactamente el mismo alto
de línea. Si el ajuste automático se calculó con esa medida provisional,
podía quedarse corto en cuanto la fuente de verdad entraba en juego —
notándose sobre todo justo después de cargar la página o justo al cambiar
de idioma. Ahora, en cuanto el navegador avisa de que las fuentes ya
cargaron del todo (`document.fonts.ready`, en `SECCION: MOTOR`), se vuelve a
calcular el tamaño con la medida definitiva. Si esto no fuera suficiente y
alguna página sigue viéndose con texto cortado en un idioma concreto,
dime la página y el idioma exactos y lo reviso puntualmente.

**Sin barra de scroll cuando el texto cabe justo.** Aunque una página se
encoja hasta caber exactamente (sin sobrar ni un píxel), `SECCION:
AUTOAJUSTE` le quita el `overflow-y: auto` en cuanto confirma que ya no hace
falta. Si no se hiciera esto, algunos sistemas (Windows, o macOS con la
opción "mostrar barras de scroll: siempre" activada) dibujan una barra fija
aunque no haya nada que desplazar. Solo si una página de verdad no cupiera
ni al tamaño mínimo, se deja el scroll activo — mejor poder desplazarse que
perder texto en silencio.

```js
var FIT = {
  step:      0.5,   // en que saltos se prueba el tamaño, en px
  growLimit: 0,      // 0 = tamaño uniforme; solo encoge, nunca agranda
  shrinkMax: 13,      // lo maximo que se le permite encoger si viene justa, en px
  floor:     11,      // nunca por debajo de esto, pase lo que pase
};
```

Después de cambiar cualquiera de estos números, prueba las páginas con más
texto (el reparto de basura, los horarios, los hoteles) en un móvil pequeño
para confirmar que ninguna se pasa.

### Saltar de página al hacer clic en el índice

Tanto en la página 2 como en el menú lateral "Contents", tocar un capítulo o
un subíndice te lleva directo a esa página — los dos funcionan igual. Si
añades o mueves páginas (`SECCION: ESTRUCTURA`), no hace falta tocar nada de
esto: el salto usa el número de página del propio índice.

---

## Cómo publicarlo en GitHub Pages

```bash
git add .
git commit -m "Actualizar textos"
git push origin main
```

En **Settings → Pages** del repositorio, elige la rama `main` y la carpeta que
contenga este `index.html`.

Todas las rutas son relativas (`img/p1.jpg`), así que funciona igual en la raíz
del dominio que en una subcarpeta. No hace falta configurar ninguna ruta base.

Si después de subir no ves los cambios, es la caché del navegador:
**Ctrl+Shift+R** (Cmd+Shift+R en Mac).

---

## Preguntas frecuentes

**Cambié un texto y ahora no carga nada.**
Falta una comilla o una coma. F12 → Console te dice la línea.

**Los caracteres japoneses o birmanos salen como cuadrados.**
El dispositivo no tiene esa tipografía. Añade la fuente Noto correspondiente al
enlace de Google Fonts del `<head>`.

**Quiero añadir un idioma nuevo.**
Copia el bloque entero de `CONTENT.en` (desde `IDIOMA: ENGLISH` hasta donde
empieza el siguiente idioma), pégalo debajo, cambia `CONTENT.en` por
`CONTENT.pt`, traduce el contenido, y añade `'pt'` a la lista `LANGS` que está
al principio de `SECCION: MOTOR`.

**¿Puedo volver a separar los archivos?**
Sí. El CSS está entero dentro de `<style>` y el JS dentro de `<script>`; sacarlos
a `book.css` y `book.js` es cortar y pegar.
