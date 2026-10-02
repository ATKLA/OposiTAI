# OposiTAI

Organizador de estudio para la oposición **TAI 2026** (Técnico Auxiliar de Informática de la Administración del Estado). Sirve para planificar y registrar el avance: qué temas llevas leídos en primera lectura, en qué orden quieres leerlos, qué material tienes de cada uno y si, al ritmo de páginas al día que te marques, llegas al plazo.

No contiene el temario ni material de estudio: solo la lista de títulos de los temas como referencia para marcar el progreso.

Una sola página HTML, sin dependencias ni proceso de build. El progreso se guarda en el navegador.

## Funcionalidades

- **Índice de temas**: los títulos de los 33 temas de la convocatoria, agrupados en sus 4 bloques (Organización del Estado, Tecnología básica, Desarrollo de sistemas, Sistemas y comunicaciones), como lista sobre la que marcar el avance.
- **Tabla de temas con cuatro columnas**: **1ª lectura**, **Temario digital**, **Fichas** e **Impreso** (icono de impresora). La 1ª lectura guarda la fecha en que se marcó y el tema leído se tiñe con el color del bloque. Digital, fichas e impreso solo se marcan o desmarcan.
- **Colores por bloque**: dentro de cada bloque todo lo marcado usa tonos de su color (verde, naranja, rosa, violeta), de más a menos intenso: 1ª lectura, digital, fichas e impreso. También las filas leídas, el "vas por aquí", la barra de progreso, el anillo de su pestaña y el confeti.
- **Orden de lectura**: cada tema se arrastra por su asa (⋮⋮) para colocarlo donde quieras dentro de su bloque; funciona con ratón y en pantallas táctiles. Con el asa enfocada, las flechas arriba y abajo lo mueven una posición. El orden se guarda por bloque y los temas conservan su número oficial y su progreso. Si el bloque abierto tiene un orden propio, junto al título "Bloques" aparece "Orden original del bloque…", que lo deshace.
- **Páginas de cada tema**, bajo su título (se guardan al pulsar Intro o salir del campo). La tarjeta de cada bloque suma las páginas leídas en 1ª lectura frente a las anotadas.
- **Plazo de la 1ª lectura**, a todo el ancho:
  - **En páginas**: dos anillos concéntricos (fuera, páginas leídas; dentro, plazo consumido: si el de fuera va por delante, llegas), una frase que dice si al ritmo del plan (20 págs/día por defecto) llegas a la fecha límite y con cuánto margen o retraso, debajo lo que da tu ritmo real (en rojo si con él no llegas), y tres cifras: fecha de fin al ritmo del plan, tu ritmo real en págs/día frente al del plan y las págs/día necesarias para llegar justo. Debajo, las páginas leídas por semana (de lunes a domingo) de toda la fase, con la línea del plan (ritmo × 7). Si la fase aún no ha empezado: cuándo empieza, su duración y los días que necesitarás al ritmo del plan. Sin páginas anotadas, pide que las anotes.
- **Fecha límite editable** con el lápiz.
- **Ritmo del plan editable** ("Plan 20 págs/día", con el lápiz), entre 1 y 500 páginas al día. Al cambiarlo se recalcula todo: si llegas, la fecha de fin, lo que necesitas y la línea de las barras semanales.
- **"Vas por aquí"**: una marca por bloque, así que se puede ir por el tema 2 del bloque I y a la vez por el tema 7 del bloque IV. Pulsando el número de un tema se marca como punto actual de su bloque (sustituye la marca anterior de ese bloque; pulsarlo otra vez la quita). Cada marca aparece en la pestaña del bloque con su número. Se quita sola al marcar ese tema como leído.
- **Página por la que vas**: en el tema marcado aparece un campo "Vas por la pág." (de 1 al total de páginas del tema). Esas páginas ya cuentan en el progreso y en el ritmo de la 1ª lectura, en la semana en que las anotas. Al marcar el tema como leído, la marca se quita y pasa a contar el tema entero.
- **Progreso por bloque**: tarjetas con anillo de progreso de la 1ª lectura, temas leídos y páginas. La tabla del bloque sale directamente de su tarjeta, sin cabecera intermedia; en escritorio una flecha del color del bloque las une.
- **Enlaces** a INAP (proceso selectivo) y THEA, centrados en la barra superior.
- **Varias pestañas**: si la app está abierta en dos pestañas, los cambios de una se reflejan en la otra.
- **Modo claro / oscuro**: sigue al sistema por defecto; el botón de la barra superior fija uno u otro.
- **Responsive**: tabla con columnas en escritorio, filas apiladas en móvil.
- **Accesibilidad**: pestañas navegables con flechas, `aria-pressed` en los checks, orden de temas con teclado, foco visible, textos alternativos en barras de progreso y respeto de `prefers-reduced-motion`.

## Uso

Abrir `index.html` en el navegador. No necesita servidor.

Para publicarlo con GitHub Pages: *Settings → Pages → Deploy from a branch*, rama `main`, carpeta `/ (root)`.

## Estructura

```
.
├── index.html   # HTML, CSS y JS en un solo archivo
└── README.md
```

## Personalización

Todo se edita dentro del `<script>` de `index.html`.

| Qué | Dónde |
| --- | --- |
| Títulos de temas y bloques | constante `DATA` |
| Fecha límite por defecto | constante `PHASES` (`deadline` en formato `AAAA-MM-DD`) |
| Ritmo del plan por defecto | constante `PPD` (20 págs/día) |
| Color de la 1ª lectura (panel de plazo) | variable CSS `--c-a` en `:root` (y su equivalente en modo oscuro) |
| Color de cada bloque | variables CSS `--b1`…`--b4` (base), `--bN-ink` y `--bN-soft`; los tonos de cada columna salen de ellas (`--t-a`, `--t-dig`, `--t-fch`, `--t-imp`). Confeti del bloque en la constante `BLK_FX` |
| Degradados y confeti | constante `THEME` |

La fecha límite y el ritmo guardados desde la interfaz tienen prioridad sobre `PHASES` y `PPD`.

## Cómo se calcula el ritmo

Todo se mide en **páginas**, no en temas: un tema de 60 páginas no pesa lo mismo que uno de 15.

- La 1ª lectura empieza el día de la primera visita.
- **Páginas del temario** = suma de las páginas anotadas. Los temas sin páginas cuentan con la media de los que sí las tienen (se indica cuántos son).
- **Plan**: las páginas al día que elijas (20 por defecto). **Acabarías** = hoy (o el inicio de la fase, si aún no ha empezado) + páginas pendientes / ritmo del plan. Si esa fecha es anterior o igual a la fecha límite, llegas; la diferencia es el margen.
- **Necesario** = páginas pendientes / días que quedan hasta la fecha límite.
- **Tu ritmo** = páginas leídas / días transcurridos (hoy incluido). Se muestra a partir del 7º día; antes, las páginas leídas esta semana (lunes a domingo) frente a las del plan (ritmo × 7). Con tu ritmo real también se calcula la fecha en que acabarías.
- Las barras semanales suman las páginas de los temas marcados cada semana, según la fecha guardada en cada marca; las marcas sin fecha cuentan en la semana de inicio de la fase.

## Datos

No hay backend. Todo se guarda en `localStorage`, por lo que el progreso es propio de cada navegador y dispositivo, y se pierde si se borran los datos del sitio.

| Clave | Contenido |
| --- | --- |
| `tai2026-progress-v1` | Estado de cada tema: 1ª lectura con su fecha (`a` / `da`), páginas (`pg`), temario digital (`dig`), fichas (`fch`) e impreso (`imp`). Los campos de 2ª lectura y resumen (`b`, `r`, `db`, `dr`) de versiones anteriores se conservan, pero ya no se muestran |
| `tai2026-deadlines` | Fecha límite editada (`{ a }`) |
| `tai2026-pace` | Ritmo del plan en págs/día (`{ a }`) |
| `tai2026-order` | Orden de lectura personalizado por bloque, con los números oficiales de los temas (`{ b4: [7, 1, 2, …] }`). Un bloque sin entrada usa el orden oficial |
| `tai2026-start` | Fecha de inicio (primera visita) |
| `tai2026-here-v2` | Tema marcado como "vas por aquí" en cada bloque y, si la anotas, la página por la que vas (`{ b1: { n, since, pag, pd }, … }`). La clave antigua `tai2026-here` se migra sola |
| `tai2026-ui-v5` | Último bloque abierto |
| `tai2026-theme` | Tema claro u oscuro elegido |

## Tecnología

HTML, CSS y JavaScript sin frameworks. Tipografías Inter, Inter Tight e IBM Plex Mono desde Google Fonts. Confeti dibujado en `<canvas>`.

## Autora

Laura Ateca Vega · [ATKLA](https://github.com/ATKLA) · [atkla.com](https://atkla.com)
