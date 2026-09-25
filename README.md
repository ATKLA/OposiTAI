# OposiTAI

Organizador de estudio para la oposición **TAI 2026** (Técnico Auxiliar de Informática de la Administración del Estado). Sirve para planificar y registrar el avance: qué temas llevas leídos en primera y segunda vuelta, cuáles tienes resumidos y si vas a buen ritmo para llegar a los plazos que te marques.

No contiene el temario ni material de estudio: solo la lista de títulos de los temas como referencia para marcar el progreso.

Una sola página HTML, sin dependencias ni proceso de build. El progreso se guarda en el navegador.

## Funcionalidades

- **Índice de temas**: los títulos de los 33 temas de la convocatoria, agrupados en sus 4 bloques (Organización del Estado, Tecnología básica, Desarrollo de sistemas, Sistemas y comunicaciones), como lista sobre la que marcar el avance.
- **Tres fases por tema**: 1ª lectura, 2ª lectura y resumen. Marcar la 2ª lectura marca también la 1ª, y quitar la 1ª quita también la 2ª. Cada marca guarda la fecha en que se hizo.
- **Plazos con ritmo**: cada lectura tiene fecha límite. La tarjeta muestra días restantes, temas por semana necesarios y una marca de "hoy deberías llevar N" sobre la barra de progreso.
- **Estado frente al plazo**: al día, adelantada, por detrás (aviso a partir de 1 tema, alerta a partir de 3) o plazo vencido.
- **Fechas límite editables** desde la propia tarjeta. La 1ª lectura tiene que acabar antes que la 2ª.
- **"Vas por aquí"**: una marca por bloque, así que se puede ir por el tema 2 del bloque I y a la vez por el tema 7 del bloque IV. Pulsando el número de un tema se marca como punto actual de su bloque (sustituye la marca anterior de ese bloque; pulsarlo otra vez la quita). Cada marca aparece en la pestaña del bloque con su número y en la tarjeta de la fase activa, con enlace directo al tema. Se quita sola al marcar ese tema como leído.
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
- **Temas esperados hoy** = `floor(33 × días transcurridos / días totales de la fase)`.
- **Ritmo necesario** = temas pendientes / semanas que quedan hasta la fecha límite.

## Datos

No hay backend. Todo se guarda en `localStorage`, por lo que el progreso es propio de cada navegador y dispositivo, y se pierde si se borran los datos del sitio. Para pasarlo a otro dispositivo o guardarlo, usa **Exportar copia** / **Importar copia** en el pie de página.

| Clave | Contenido |
| --- | --- |
| `tai2026-progress-v1` | Estado de cada tema (lecturas, resumen y fechas) |
| `tai2026-deadlines` | Fechas límite editadas |
| `tai2026-start` | Fecha de inicio (primera visita) |
| `tai2026-here-v2` | Tema marcado como "vas por aquí" en cada bloque (`{ b1: { n, since }, … }`). La clave antigua `tai2026-here` se migra sola |
| `tai2026-ui-v5` | Último bloque abierto |
| `tai2026-theme` | Tema claro u oscuro elegido |

## Tecnología

HTML, CSS y JavaScript sin frameworks. Tipografías Inter, Inter Tight e IBM Plex Mono desde Google Fonts. Confeti dibujado en `<canvas>`.

## Autora

Laura Ateca Vega · [ATKLA](https://github.com/ATKLA) · [atkla.com](https://atkla.com)
