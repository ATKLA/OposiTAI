# OposiTAI

Organizador de estudio para la oposición **TAI 2026** (Técnico Auxiliar de Informática de la Administración del Estado). Sirve para planificar y registrar el avance: qué temas llevas leídos en primera y segunda vuelta, cuáles tienes resumidos, qué material tienes de cada uno y si, al ritmo de páginas al día que te marques, llegas a los plazos.

No contiene el temario ni material de estudio: solo la lista de títulos de los temas como referencia para marcar el progreso.

Una sola página HTML, sin dependencias ni proceso de build. El progreso se guarda en el navegador.

## Funcionalidades

- **Índice de temas**: los títulos de los 33 temas de la convocatoria, agrupados en sus 4 bloques (Organización del Estado, Tecnología básica, Desarrollo de sistemas, Sistemas y comunicaciones), como lista sobre la que marcar el avance.
- **Tres pasos por tema**: 1ª lectura, resumen y 2ª lectura (del resumen). La 1ª lectura y el resumen son independientes; la 2ª lectura requiere el resumen, así que marcarla marca el resumen y quitar el resumen quita la 2ª. Cada marca guarda la fecha en que se hizo.
- **Material de cada tema**, bajo su título: número de páginas (se guarda al pulsar Intro o salir del campo) y dos marcas, **Digital** (tengo el temario digital) y **Fichas**, que solo se marcan o desmarcan. La cabecera de cada bloque suma las páginas anotadas y las leídas en 1ª lectura.
- **Plazos en un panel con pestañas** (1ª lectura, Resúmenes, 2ª lectura), a todo el ancho:
  - **Lecturas**, en páginas: dos anillos concéntricos (fuera, páginas leídas; dentro, plazo consumido: si el de fuera va por delante, llegas), una frase que dice si al ritmo del plan (20 págs/día por defecto) llegas a la fecha límite y con cuánto margen o retraso, debajo lo que da tu ritmo real (en rojo si con él no llegas), y tres cifras: fecha de fin al ritmo del plan, tu ritmo real en págs/día frente al del plan y las págs/día necesarias para llegar justo. Debajo, las páginas leídas por semana (de lunes a domingo) de toda la fase, con la línea del plan (ritmo × 7). Si la fase aún no ha empezado: cuándo empieza, su duración y los días que necesitarás al ritmo del plan. Sin páginas anotadas, pide que las anotes.
  - **Resúmenes**: anillo de progreso y un cuadro por tema agrupado por bloque; pulsarlo lleva al tema.
- **Fechas límite editables** desde la pestaña de cada lectura. La 1ª lectura tiene que acabar antes que la 2ª.
- **Ritmo del plan editable** ("Plan 20 págs/día", con el lápiz) desde la pestaña de cada lectura: cada lectura tiene el suyo, entre 1 y 500 páginas al día. Al cambiarlo se recalcula todo: si llegas, la fecha de fin, lo que necesitas y la línea de las barras semanales.
- **"Vas por aquí"**: una marca por bloque, así que se puede ir por el tema 2 del bloque I y a la vez por el tema 7 del bloque IV. Pulsando el número de un tema se marca como punto actual de su bloque (sustituye la marca anterior de ese bloque; pulsarlo otra vez la quita). Cada marca aparece en la pestaña del bloque con su número. Se quita sola al marcar ese tema como leído.
- **Página por la que vas**: en el tema marcado aparece un campo "Vas por la pág." (de 1 al total de páginas del tema). Esas páginas ya cuentan en el progreso y en el ritmo de la lectura que el tema tiene pendiente (la 1ª si aún no está leído, la 2ª si ya lo está), en la semana en que las anotas. Al marcar el tema como leído, la marca se quita y pasa a contar el tema entero.
- **Progreso por bloque**: pestañas con anillo de progreso de la fase activa y contador de resúmenes.
- **Copia de seguridad**: exportar e importar todo el progreso en un archivo `.json` desde el pie de página.
- **Varias pestañas**: si la app está abierta en dos pestañas, los cambios de una se reflejan en la otra.
- **Modo claro / oscuro**: sigue al sistema por defecto; el botón de la barra superior fija uno u otro.
- **Responsive**: tabla con columnas en escritorio, filas apiladas en móvil.
- **Accesibilidad**: pestañas navegables con flechas, `aria-pressed` en los checks, foco visible, textos alternativos en barras de progreso y respeto de `prefers-reduced-motion`.

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
| Fechas límite por defecto | constante `PHASES` (`deadline` en formato `AAAA-MM-DD`) |
| Ritmo del plan por defecto | constante `PPD` (20 págs/día) |
| Colores de fase | variables CSS `--c-a`, `--c-b`, `--c-r` en `:root` (y sus equivalentes en modo oscuro) |
| Degradados y confeti | constante `THEME` |

Las fechas límite y el ritmo guardados desde la interfaz tienen prioridad sobre `PHASES` y `PPD`.

## Cómo se calcula el ritmo

Todo se mide en **páginas**, no en temas: un tema de 60 páginas no pesa lo mismo que uno de 15.

- La 1ª lectura empieza el día de la primera visita; la 2ª, el día en que vence la 1ª.
- **Páginas del temario** = suma de las páginas anotadas. Los temas sin páginas cuentan con la media de los que sí las tienen (se indica cuántos son).
- **Plan**: las páginas al día que elijas para cada lectura (20 por defecto). **Acabarías** = hoy (o el inicio de la fase, si aún no ha empezado) + páginas pendientes / ritmo del plan. Si esa fecha es anterior o igual a la fecha límite, llegas; la diferencia es el margen.
- **Necesario** = páginas pendientes / días que quedan hasta la fecha límite.
- **Tu ritmo** = páginas leídas / días de fase transcurridos (hoy incluido). Se muestra a partir del 7º día; antes, las páginas leídas esta semana (lunes a domingo) frente a las del plan (ritmo × 7). Con tu ritmo real también se calcula la fecha en que acabarías.
- Las barras semanales suman las páginas de los temas marcados cada semana, según la fecha guardada en cada marca; las marcas sin fecha cuentan en la semana de inicio de la fase.

## Datos

No hay backend. Todo se guarda en `localStorage`, por lo que el progreso es propio de cada navegador y dispositivo, y se pierde si se borran los datos del sitio. Para pasarlo a otro dispositivo o guardarlo, usa **Exportar copia** / **Importar copia** en el pie de página.

| Clave | Contenido |
| --- | --- |
| `tai2026-progress-v1` | Estado de cada tema: lecturas y resumen con sus fechas (`a`, `b`, `r` / `da`, `db`, `dr`), páginas (`pg`), temario digital (`dig`) y fichas (`fch`). Los campos nuevos son opcionales: el progreso y las copias anteriores se leen sin cambios |
| `tai2026-deadlines` | Fechas límite editadas |
| `tai2026-pace` | Ritmo del plan de cada lectura en págs/día (`{ a, b }`) |
| `tai2026-start` | Fecha de inicio (primera visita) |
| `tai2026-here-v2` | Tema marcado como "vas por aquí" en cada bloque y, si la anotas, la página por la que vas (`{ b1: { n, since, pag, pd }, … }`). La clave antigua `tai2026-here` se migra sola |
| `tai2026-ui-v5` | Último bloque y última pestaña de plazos abiertos |
| `tai2026-theme` | Tema claro u oscuro elegido |

## Tecnología

HTML, CSS y JavaScript sin frameworks. Tipografías Inter, Inter Tight e IBM Plex Mono desde Google Fonts. Confeti dibujado en `<canvas>`.

## Autora

Laura Ateca Vega · [ATKLA](https://github.com/ATKLA) · [atkla.com](https://atkla.com)
