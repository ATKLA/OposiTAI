# OposiTAI

Organizador de estudio para la oposición **TAI 2026** (Técnico Auxiliar de Informática de la Administración del Estado). Sirve para planificar y registrar el avance: qué temas llevas leídos en primera y segunda vuelta, cuáles tienes resumidos y si vas a buen ritmo para llegar a los plazos que te marques.

No contiene el temario ni material de estudio: solo la lista de títulos de los temas como referencia para marcar el progreso.

Una sola página HTML, sin dependencias ni proceso de build. El progreso se guarda en el navegador.

## Funcionalidades

- **Índice de temas**: los títulos de los 33 temas de la convocatoria, agrupados en sus 4 bloques (Organización del Estado, Tecnología básica, Desarrollo de sistemas, Sistemas y comunicaciones), como lista sobre la que marcar el avance.
- **Tres pasos por tema**: 1ª lectura, resumen y 2ª lectura (del resumen). La 1ª lectura y el resumen son independientes; la 2ª lectura requiere el resumen, así que marcarla marca el resumen y quitar el resumen quita la 2ª. Cada marca guarda la fecha en que se hizo.
- **Plazos en un panel con pestañas** (1ª lectura, Resúmenes, 2ª lectura), a todo el ancho:
  - **Lecturas**: dos anillos concéntricos (fuera, temas leídos; dentro, plazo consumido: si el de fuera va por delante, llegas), una frase que dice si llegas y con cuánto margen o retraso, y tres cifras: fecha estimada de fin, tu ritmo (con lo que te sobra o te falta) y el ritmo necesario. Debajo, las barras de temas por semana (de lunes a domingo, cada barra con la fecha de su lunes) de toda la fase, con la línea del ritmo necesario. Si la fase aún no ha empezado, solo anillos y cifras (cuándo empieza, duración y ritmo que necesitarás).
  - **Resúmenes**: anillo de progreso y un cuadro por tema agrupado por bloque; pulsarlo lleva al tema.
- **Fechas límite editables** desde la pestaña de cada lectura. La 1ª lectura tiene que acabar antes que la 2ª.
- **"Vas por aquí"**: una marca por bloque, así que se puede ir por el tema 2 del bloque I y a la vez por el tema 7 del bloque IV. Pulsando el número de un tema se marca como punto actual de su bloque (sustituye la marca anterior de ese bloque; pulsarlo otra vez la quita). Cada marca aparece en la pestaña del bloque con su número. Se quita sola al marcar ese tema como leído.
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
| Colores de fase | variables CSS `--c-a`, `--c-b`, `--c-r` en `:root` (y sus equivalentes en modo oscuro) |
| Degradados y confeti | constante `THEME` |

Las fechas límite guardadas desde la interfaz tienen prioridad sobre las de `PHASES`.

## Cómo se calcula el ritmo

- La 1ª lectura empieza el día de la primera visita; la 2ª, el día en que vence la 1ª.
- **Ritmo necesario** = temas pendientes / semanas que quedan hasta la fecha límite (desde hoy, o desde el inicio de la fase si aún no ha empezado).
- **Tu ritmo** = temas leídos / días de fase transcurridos (hoy incluido) × 7. La fecha estimada de fin se calcula a partir del 7º día: antes, un tema leído el primer día daría "7 temas/semana". Durante esos primeros días se muestra el objetivo de la semana en curso (lunes a domingo): temas leídos esta semana frente al ritmo necesario redondeado hacia arriba, y cuántos faltan.
- **Acabarías el** = hoy + temas pendientes / tu ritmo. El margen es la diferencia con la fecha límite.
- Las barras semanales usan la fecha guardada en cada marca; las marcas sin fecha cuentan en la semana de inicio de la fase.

## Datos

No hay backend. Todo se guarda en `localStorage`, por lo que el progreso es propio de cada navegador y dispositivo, y se pierde si se borran los datos del sitio. Para pasarlo a otro dispositivo o guardarlo, usa **Exportar copia** / **Importar copia** en el pie de página.

| Clave | Contenido |
| --- | --- |
| `tai2026-progress-v1` | Estado de cada tema (lecturas, resumen y fechas) |
| `tai2026-deadlines` | Fechas límite editadas |
| `tai2026-start` | Fecha de inicio (primera visita) |
| `tai2026-here-v2` | Tema marcado como "vas por aquí" en cada bloque (`{ b1: { n, since }, … }`). La clave antigua `tai2026-here` se migra sola |
| `tai2026-ui-v5` | Último bloque y última pestaña de plazos abiertos |
| `tai2026-theme` | Tema claro u oscuro elegido |

## Tecnología

HTML, CSS y JavaScript sin frameworks. Tipografías Inter, Inter Tight e IBM Plex Mono desde Google Fonts. Confeti dibujado en `<canvas>`.

## Autora

Laura Ateca Vega · [ATKLA](https://github.com/ATKLA) · [atkla.com](https://atkla.com)
