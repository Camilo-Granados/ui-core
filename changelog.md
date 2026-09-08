# Changelog

## v0.5.6
- Change: `.icon-l` de `2.5rem` a `2.25rem` para reducir el tamaño de los iconos grandes.
- Change: `--line-height-2xl` de `1.2` a `1.1` para compactar la tipografía de mayor tamaño.
- Change: `--font-size-m` y `--font-size-l` aumentan a `17px` y `19px`, respectivamente, desde 1200 px.

## v0.5.5
- Change: tokens `--font-size-s` a `--font-size-2xl` y `--line-height-s` a `--line-height-2xl` en `assets/css/typography.css` para aplicar la escala tipográfica responsiva y alturas relativas.
- Add: tablas de tamaños y alturas de línea responsivos de `--font-size-s` a `--font-size-2xl` en `docs/DESIGN.md`.

## v0.5.4
- Add: `layouts/partials/icons/language.html`.
- Add: utilidad `.skip-link` en `assets/css/utility.css` para revelar enlaces de salto al recibir foco visible.
- Add: utilidad `.visually-hidden` en `assets/css/utility.css` para conservar contenido accesible y revelar controles internos al recibir foco.
- Change: `--duration-fast`, `--duration-base` y `--duration-slow` reducen su duración bajo `prefers-reduced-motion: reduce`.

## v0.5.3
- Fix: publicación completa del módulo; reemplaza la publicación incompleta de v0.5.2.

## v0.5.2
- Add: clases card-bg y card-no-bg

## v0.5.1
- Add: `layouts/partials/icons/calendar.html`

## v0.4.0
- Add: animations.css

## v0.3.7
- Add: reset de button, input, select, textarea

## v0.3.6
- Add: clase ilus-l para tamaño de ilustraciones
- Change: clase ilus-m de 4px a 3px
- Add: ilustración teamwork

## v0.3.5
- Add: clase ilus-l para tamaño de ilustraciones
- Fix: variables de color en ilustraciones existentes

## v0.3.4
- Add: variables 'ink' solo para ilustraciones

## v0.3.3
- Add: colores dark theme especiales para class ilus
- Add: 2 ilustraciones

## v0.3.2
- Add: En sizes.css, agregar variables stroke-s: 2px y stroke-m: 4px


## v0.3.1
- Add: clase ilus-xl para tamaño de ilustraciones

## v0.3.0
- Add: `layouts/partials/illustrations`

## v0.2.4
- Remove (non breaking): Quitar de reset.css html "scrollbar-gutter: stable;"

## v0.2.3
- Add: clase col-1, col-2 y col-3
- Add: --font-weight-light (300)
- Add: --font-weight-medium (520)
- Change: --font-weight-semibold (560)
- Change: --font-weight-bold (650)
- Add: --border-highlight (mora-40)

## v0.2.2
- Add: clase grid-align-start para evitar que los elementos sean full height

## v0.2.1
- Add: nuevo tamaño de icono icon-xs (16px)
- Change: peso de tipografías para mejor renderizado en celular
- Add: --font-weight-bold (700)

## v0.2.0
- Add: iconos en `layouts/partials/icons`
- Remove (breaking): `layouts/partials/gtm` → ahora debe gestionarse por cada sitio

## v0.1.1
- Remove (breaking): `strong` y `p` del `reset.css` para delegarlo al sitio

## v0.1.0
- Initial release con CSS base y partial GTM
