# Reglas de diseño

Este documento reúne las reglas que guían el sistema visual del módulo `ui-core`.

## Alcance del módulo

- El módulo solo genera tokens, utilidades y assets genéricos.
- Su propósito es estandarizar el lenguaje de diseño a través de los sitios que lo consumen.
- No debe incluir componentes, contenido ni decisiones específicas de un sitio consumidor.

## Color y temas

- El tema oscuro se define modificando los **tokens primitivos** de color.
- No se deben crear ni sobrescribir tokens semánticos específicos para el tema oscuro.
- Los tokens semánticos deben conservar su intención y tomar sus valores de los tokens primitivos activos.

### Tokens de color existentes

- **Paleta primitiva:** `--mora-20` a `--mora-98` y `--gris-20` a `--gris-100` forman las escalas principales. `--menta`, `--pitaya` y `--amarillo` son colores de acento. Estos valores cambian bajo `prefers-color-scheme: dark`.
- **Superficies y controles:** los tokens `--bg-*` describen fondos de superficie y de botón primario para sus estados.
- **Contenido y bordes:** `--text-*` define texto, enlaces, iconos y botones; `--border-*` define divisores, pestañas, imágenes, destacados y botón secundario.
- **Ilustraciones:** `--ink-*` expone colores reutilizables para ilustraciones SVG, mientras `--ink-line-drawing` y `--ink-cloth-4` describen roles concretos dentro de ellas.

## Tipografía

- `--font-family-headline` y `--font-family-body` definen las familias para titulares y cuerpo; actualmente ambas usan Readex Pro con reserva `sans-serif`.
- `--font-weight-light`, `--font-weight-regular`, `--font-weight-medium`, `--font-weight-semibold` y `--font-weight-bold` establecen la escala de pesos.
- `--font-size-s` a `--font-size-2xl` y sus equivalentes `--line-height-s` a `--line-height-2xl` forman la escala tipográfica. Sus valores aumentan de forma responsiva desde 810 px y, para los tamaños, desde 1200 px.

## Tamaños y espaciado

- `--size-3xs` a `--size-2xl` es la escala primitiva de medidas, de 4 px a 80 px.
- `--space-2xs` a `--space-2xl` traduce esa escala en espaciado semántico y aumenta sus valores mayores desde 810 px.
- `--border-radius-s` y `--border-radius-button` definen radios; `--stroke-s`, `--stroke-m` y `--stroke-l` definen grosores de trazo.
- `--hero-v-padding` establece el espaciado vertical del hero y se ajusta desde 810 px. El token de footer está definido como `--foter-v-padding` en la base y como `--footer-v-padding` desde 810 px; deben unificarse antes de usarse como token compartido.

## Accesibilidad

- El módulo debe cumplir el nivel **WCAG 2.2 AA** como mínimo.
- Mantén contraste suficiente entre texto, iconos, controles y sus fondos.
- Todo componente interactivo debe ser operable con teclado y conservar un foco visible.
- El HTML debe usar elementos semánticos, nombres accesibles adecuados y una jerarquía de encabezados coherente.
- Las animaciones deben respetar la preferencia de movimiento reducido del sistema.

Consulta [Reducción de movimiento](reduced-motion.md) para usar los tokens de
duración de `assets/css/animations.css` y para implementar controles de pausa
en animaciones continuas.

## Iconografía

- Solo se usarán iconos SVG creados como parciales de Hugo dentro de `layouts/partials/icons/`.
- No se usarán bibliotecas de iconos, iconos remotos, imágenes rasterizadas ni SVG insertados desde fuentes externas para representar iconografía de interfaz.
- Los iconos deben ser decorativos por defecto; cuando comuniquen información o actúen como control, deben incluir una alternativa textual accesible.

## Utilidades existentes

Las utilidades residen en `assets/css/utility.css` y componen patrones genéricos sobre los tokens del sistema.

- **Visibilidad:** `.desktop-only` y `.mobile-only` alternan contenido desde el breakpoint de 810 px.
- **Accesibilidad:** `.skip-link` oculta visualmente un enlace de salto hasta que recibe foco visible. Consulta [Enlaces de salto](skip-links.md) para el marcado de adopción.
- **Contenido asistivo:** `.visually-hidden` conserva contenido para tecnologías asistivas y revela controles internos al recibir foco. Consulta [Contenido solo para tecnologías asistivas](visually-hidden.md) para sus límites de uso.
- **Layout flex:** `.flex-row` dispone el contenido en columna en móvil y en fila desde 810 px; `.flex-col` crea una columna alineada al inicio; `.space-between` reparte los elementos en el eje principal.
- **Layout grid:** `.grid-2-cards`, `.grid-3-cards` y `.grid-12` definen composiciones de tarjetas y una retícula de doce columnas. `.col-1` a `.col-9` y `.col-12` determinan el número de columnas ocupadas, mientras `.grid-align-start` alinea los ítems al inicio.
- **Separación:** `.gap-2xs`, `.gap-xs`, `.gap-s`, `.gap-m`, `.gap-l` y `.gap-xl` aplican separación basada en tokens; `.gap-xl` aumenta en pantallas desde 810 px.
- **Texto:** `.text-primary`, `.text-secondary` y `.text-highlight` aplican color semántico. `.text-semibold` define peso semibold y `.text-s` a `.text-2xl` aplican pares de tamaño y altura de línea tipográficos.
- **Secciones:** `.highlight` aporta una superficie destacada; `.spacer-2xl` crea separación vertical; `.divider` muestra un divisor; `.card-bg` y `.card-no-bg` ofrecen variantes genéricas de tarjeta.
- **Recursos visuales:** `.icon-xs` a `.icon-l` fijan tamaños de iconos SVG; `.ilus-l` y `.ilus-xl` limitan el tamaño de ilustraciones y escalan desde 810 px.
