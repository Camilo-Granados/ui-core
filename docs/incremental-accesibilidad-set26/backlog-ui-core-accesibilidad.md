# Backlog — accesibilidad transversal

## Propósito

Incorporar en `ui-core` patrones reutilizables de accesibilidad para que los
sitios consumidores no tengan que resolver de forma independiente el acceso
directo al contenido y la interacción de los menús móviles. El módulo debe
ofrecer estilos, documentación y, si procede, comportamiento opcional; cada
sitio conserva su estructura, textos, rutas y decisiones de marca.

## Alcance y principios

- Cumplir WCAG 2.2 nivel AA, en particular 2.1.1, 2.4.1, 2.4.3, 2.4.7 y 4.1.2.
- Usar tokens semánticos de `ui-core`; no fijar colores, espaciados ni puntos
  de quiebre propios de un consumidor.
- No imponer un layout de cabecera, una taxonomía de navegación ni textos de
  interfaz.
- Mantener una experiencia funcional sin JavaScript. El comportamiento con
  JavaScript debe mejorar la interacción, no ocultar navegación esencial.
- No duplicar el estilo global existente de `:focus-visible`.

## 1. Utilidad para enlaces de salto — Completado

**Prioridad: alta**

**Entregado:** `.skip-link` en `assets/css/utility.css` y guía de adopción en
[`docs/skip-links.md`](../skip-links.md).

Añadir una clase de utilidad `.skip-link` para aplicar a enlaces que llevan al
contenido principal u otro destino de salto.

### Contrato

- La clase solo resuelve la presentación. El consumidor aporta el elemento
  `<a>`, el texto localizado y el destino del enlace.
- El enlace debe estar visualmente oculto mientras no recibe foco y mostrarse
  con `:focus-visible`.
- Al mostrarse, debe quedar por encima de cabeceras o elementos fijos y no
  provocar desplazamiento de la página.
- Los colores, fondo, espaciado y radio deben proceder de tokens semánticos.

### Criterios de aceptación

- Con teclado, el primer foco puede revelar el enlace y su texto es legible.
- El enlace conserva el indicador global de foco visible de `ui-core`.
- Al perder foco, el enlace deja de interferir visualmente con la interfaz.
- La utilidad funciona con una cabecera fija sin asumir su altura ni su
  `z-index` concreto.

## 2. Patrón documentado de menú móvil accesible

**Prioridad: media**

Documentar un contrato de implementación para menús móviles desplegables. La
documentación debe ser independiente de nombres de clases y de cualquier
estructura visual concreta.

### Requisitos del patrón

- Un botón controla el panel mediante `aria-controls` y comunica su estado con
  `aria-expanded`.
- El nombre accesible del botón describe la acción o el estado actual, por
  ejemplo «Abrir menú» y «Cerrar menú».
- Cuando el panel móvil está cerrado, su contenido se excluye del orden de
  tabulación y del árbol de accesibilidad mediante `inert` o una alternativa
  equivalente.
- Al abrir, el foco llega al primer elemento interactivo del panel.
- Escape cierra el panel y devuelve el foco al control que lo abrió.
- Al pasar a un punto de quiebre donde la navegación es persistente, esta debe
  recuperar interactividad y visibilidad aunque el panel estuviera cerrado en
  móvil.
- Sin JavaScript, la navegación debe permanecer disponible.

### Criterios de aceptación

- El ejemplo documentado se puede recorrer enteramente con teclado.
- No hay enlaces ocultos visualmente que puedan recibir foco en el estado
  móvil cerrado.
- Los atributos ARIA, el estado visual y el foco se actualizan como una única
  transición de estado.
- La documentación indica cómo adaptar las etiquetas a la localización del
  consumidor.

## 3. Controlador opcional de menú móvil

**Prioridad: media — investigar antes de implementar**

Evaluar la inclusión de un controlador JavaScript pequeño, sin dependencias y
optativo, que materialice el patrón anterior. Solo debe añadirse si ofrece una
API estable para distintas estructuras de navegación; de lo contrario, la
documentación será la entrega de esta tarea.

### API mínima propuesta

El controlador debe aceptar referencias a:

- botón disparador;
- panel controlado;
- primer destino de foco opcional;
- consulta de medio o punto de quiebre que define el modo móvil;
- etiquetas localizadas para los estados abierto y cerrado.

### Comportamiento requerido

- Sincronizar clase o atributo visual, `aria-expanded`, nombre accesible,
  `inert` y bloqueo de desplazamiento configurado por el consumidor.
- Enfocar el primer destino al abrir y restaurar el foco al disparador al
  cerrar mediante Escape.
- Recalcular el estado al cambiar entre los modos móvil y persistente.
- No inicializar ni modificar componentes que el consumidor no haya registrado
  explícitamente.

### Criterios de aceptación

- El controlador no requiere un framework ni introduce efectos globales salvo
  los configurados por el consumidor.
- El panel es inaccesible por teclado y tecnologías asistivas cuando está
  cerrado en móvil, y accesible en el modo persistente.
- La API permite usar etiquetas en cualquier idioma.
- Se incluyen pruebas automatizadas de cambios de estado, Escape, foco y
  cambio de punto de quiebre, además de instrucciones de validación manual.

## Entregables

1. La utilidad `.skip-link`, acompañada de una página de uso mínima.
2. La documentación del patrón de menú móvil accesible.
3. Una decisión registrada sobre el controlador opcional: implementar con API
   documentada y pruebas, o posponerlo con su justificación.
4. Notas de versión que indiquen los requisitos de adopción para consumidores.

## Referencia provisional de un consumidor

Los siguientes fragmentos muestran una implementación local que cumple el
contrato propuesto. Son material de referencia para orientar la solución del
módulo; no definen nombres de clases, textos, puntos de quiebre ni una API
obligatoria para `ui-core`.

### Enlace de salto y destino

El consumidor coloca el enlace al inicio de `body` y hace enfocable el destino
sin añadirlo al orden de tabulación normal:

```html
<body>
  <a class="skip-link" href="#content">Saltar al contenido principal</a>
  <!-- Cabecera y navegación -->
  <main id="content" tabindex="-1">
    <!-- Contenido principal -->
  </main>
</body>
```

La regla visual candidata a convertirse en utilidad del módulo es:

```css
.skip-link {
  position: fixed;
  top: var(--space-xs);
  left: var(--space-xs);
  z-index: 1300;
  padding: var(--space-2xs) var(--space-xs);
  background-color: var(--bg-surface-default);
  color: var(--text-primary);
  transform: translateY(-200%);
}

.skip-link:focus-visible {
  transform: translateY(0);
}
```

El módulo debe sustituir el valor de `z-index` por un mecanismo compatible con
su propia escala de capas, si existe, y mantener el foco visible global que ya
proporciona.

### Marcado mínimo del menú

Este ejemplo ilustra las relaciones ARIA necesarias; los textos deben
localizarse en cada consumidor:

```html
<button
  class="menu-toggle"
  aria-label="Abrir menú"
  aria-controls="main-menu-list"
  aria-expanded="false">
  <!-- Icono decorativo proporcionado por el sistema de iconos -->
</button>

<div id="main-menu-list" class="menu-list">
  <a href="/proyectos">Proyectos</a>
  <a href="/proceso">Proceso</a>
</div>
```

### Máquina de estado local del menú

La función siguiente concentra las actualizaciones que deben ocurrir juntas:

```js
const setMenuState = (open, { returnFocus = false } = {}) => {
  menuOpen = open;

  menu.classList.toggle("is-open", menuOpen);
  toggle.classList.toggle("is-open", menuOpen);
  toggle.setAttribute("aria-expanded", String(menuOpen));
  toggle.setAttribute("aria-label", menuOpen ? "Cerrar menú" : "Abrir menú");
  menu.inert = mobileBreakpoint.matches && !menuOpen;
  document.body.classList.toggle("no-scroll", mobileBreakpoint.matches && menuOpen);

  if (menuOpen) {
    firstMenuLink?.focus();
  } else if (returnFocus) {
    toggle.focus();
  }
};
```

El consumidor vincula la apertura y Escape a esa transición:

```js
toggle.addEventListener("click", () => setMenuState(!menuOpen));

document.addEventListener("keydown", (event) => {
  if (event.key === "Escape" && menuOpen && mobileBreakpoint.matches) {
    setMenuState(false, { returnFocus: true });
  }
});

mobileBreakpoint.addEventListener("change", () => {
  if (!mobileBreakpoint.matches) {
    setMenuState(false);
  } else {
    setMenuState(menuOpen);
  }
});
```

En una implementación de módulo, el bloqueo de desplazamiento y las clases
visuales deben ser opciones del consumidor. El estado accesible (`inert`,
atributos ARIA y foco) no debe depender de esos detalles de presentación.

## 7. Patrón opcional para enlaces externos

**Prioridad: baja**

Documentar, y evaluar si conviene implementar, un patrón reutilizable para
enlaces que abren una pestaña o ventana nueva. El módulo ya aporta el icono
decorativo `external-link`; no se requiere crear iconografía nueva.

### Requisitos del patrón

- Cuando el consumidor elija `target="_blank"`, el marcado debe incluir
  `rel="noopener noreferrer"`.
- El consumidor conserva la decisión de abrir una nueva pestaña, el destino y
  el texto localizado que lo anuncia; el módulo no debe fijar ningún idioma ni
  forzar ese comportamiento para todos los enlaces externos.
- El consumidor decide si el indicador se muestra visualmente o solo a
  tecnologías asistivas mediante texto localizado; el patrón debe explicar las
  implicaciones de cada alternativa.
- La solución debe funcionar sin JavaScript y no sustituir el nombre accesible
  del enlace por el del icono.

### Criterios de aceptación

- El ejemplo con nueva pestaña incluye la relación de seguridad y comunica el
  comportamiento según la política elegida por el consumidor, incluida la
  alternativa de anunciarlo solo a tecnologías asistivas.
- El ejemplo no añade aviso ni `target` cuando el enlace abre en la misma
  pestaña.
- La documentación muestra cómo aportar el aviso desde el sistema de
  localización de cada consumidor.

### Referencia provisional aplicada en portafolio.camilo-granados.com

El pie global mantiene visible únicamente el nombre del destino. El aviso de
nueva pestaña se conserva dentro del nombre accesible mediante una utilidad
local `.visually-hidden`; el texto procede de `i18n/es.toml`, por lo que no
queda fijado en el layout.

```go-html-template
<a
  class="link-discreet"
  href="https://www.linkedin.com/in/camilo-granados/"
  target="_blank"
  rel="noopener noreferrer"
>
  LinkedIn
  <span class="visually-hidden">{{ i18n "opensInNewTab" }}</span>
</a>
```

```toml
# i18n/es.toml
[opensInNewTab]
other = "(se abre en una pestaña nueva)"
```

La utilidad se incorporó localmente mientras se evalúa su entrega en el
módulo; el backlog de `ui-core` ya incluye el contrato provisional completo:

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

## 6. Contrato accesible para vídeo HTML5

**Prioridad: media**

Documentar un patrón reutilizable para el marcado de vídeo HTML5 en los sitios
consumidores. El módulo no debe aportar el contenido editorial, los archivos de
subtítulos ni decidir si un medio tiene audio, pero sí establecer el contrato y
un ejemplo de controles seguros.

### Requisitos del patrón

- Incluir controles nativos y `playsinline`; no activar `autoplay` ni `loop` de
  forma predeterminada para medios no esenciales.
- Cuando el vídeo tenga audio, el consumidor debe aportar una pista WebVTT de
  subtítulos sincronizados con idioma y etiqueta localizados.
- Cuando sea silencioso, el consumidor debe aportar desde su contenido una
  alternativa textual del vídeo visual si esta comunica información.
- Usar `preload="metadata"` como valor por defecto para evitar descargas y
  reproducción inesperadas.
- Documentar la operabilidad por teclado de los controles nativos y la
  validación manual de foco, pausa y subtítulos.

### Criterios de aceptación

- El ejemplo no comienza a reproducirse por sí solo y puede pausarse con los
  controles nativos.
- El ejemplo con audio declara una pista `kind="captions"`, `srclang` y una
  etiqueta localizada.
- La documentación separa claramente el contrato de la plantilla de las
  decisiones editoriales y de localización de cada consumidor.

### Referencia provisional implementada en portafolio.camilo-granados.com

El sitio usa Hugo, pero el marcado siguiente expresa el contrato que el módulo
puede documentar o adaptar a su propio sistema de componentes. No debe copiar
las etiquetas en español ni asumir que los consumidores usan Hugo.

```html
<video class="video" controls playsinline preload="metadata">
  <source src="video.mp4" type="video/mp4">
  <track
    kind="captions"
    src="video.es.vtt"
    srclang="es"
    label="Español"
    default>
  Tu navegador no soporta video HTML5.
</video>
```

El parcial provisional recibe la fuente, una clase opcional y los metadatos de
la pista; solo emite `track` cuando el contenido editorial declara subtítulos:

```go-html-template
<video class="{{ .class | default "video" }}" controls playsinline preload="metadata">
  <source src="{{ .src }}" type="video/mp4">
  {{ with .captions }}
  <track kind="captions" src="{{ . }}"
    srclang="{{ $.captionslang | default "es" }}"
    label="{{ $.captionslabel | default "Español" }}" default>
  {{ end }}
</video>
```

Ejemplo de decisión editorial desde un page bundle Hugo:

```md
{{< video
  src="video.mp4"
  captions="video.es.vtt"
  captionslang="es"
  captionslabel="Español"
>}}
```

La pista WebVTT también permanece junto al medio en el bundle y es parte del
contenido localizado; por ejemplo:

```vtt
WEBVTT

00:00.000 --> 00:09.913
[Música instrumental]
```

## 5. Estrategia global para reducción de movimiento — Completado

**Prioridad: media**

**Entregado:** `assets/css/animations.css` reduce las duraciones del módulo
según `prefers-reduced-motion`; la guía de adopción está en
[`docs/reduced-motion.md`](../reduced-motion.md).

Incorporar una regla global, optativa y documentada para responder a
`prefers-reduced-motion: reduce`. Debe acortar o desactivar las animaciones y
transiciones no esenciales de los componentes de `ui-core`, sin asumir las
animaciones específicas de cada sitio consumidor.

### Requisitos del patrón

- Bajo la preferencia de reducción, las apariciones basadas en opacidad o
  transformación deben mostrarse inmediatamente, sin dejar contenido oculto.
- Las transiciones no esenciales de los componentes deben ser imperceptibles o
  eliminarse; las interacciones y los indicadores de foco deben conservarse.
- El CSS no puede convertir en dependiente de JavaScript la visibilidad del
  contenido: el estado base debe ser visible y el movimiento una mejora
  progresiva activada explícitamente por el consumidor.
- Documentar cómo los consumidores pausan o desactivan animaciones continuas,
  como una marquesina, pues su semántica y control pertenecen al componente que
  las implementa.

### Criterios de aceptación

- Con `prefers-reduced-motion: reduce` no hay desplazamiento animado al cargar
  ni al revelar componentes de `ui-core`.
- Si JavaScript no carga, ningún contenido queda oculto por una clase de
  animación de `ui-core`.
- La documentación diferencia la política reutilizable de reducción de
  movimiento de los controles de pausa propios de cada componente.

### Referencia provisional aplicada en portafolio.camilo-granados.com

La siguiente regla se aplicó localmente en `assets/css/animations.css`. Es una
referencia para evaluar y adaptar a los tokens, arquitectura y alcance de
`ui-core`; no se debe copiar de forma literal si afecta componentes que
necesiten una transición funcional.

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
  }
}
```

Las apariciones se implementaron como mejora progresiva: el elemento es visible
en el CSS base y JavaScript añade `.has-motion` únicamente cuando la preferencia
no es `reduce`.

```css
.has-motion .animate-fade-up {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.has-motion .animate-fade-up.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

La pausa de una animación continua no forma parte de esta utilidad global. Como
referencia, la marquesina local usa un botón propio con `aria-pressed` y cambia
la clase de su galería:

```js
const setPaused = paused => {
  gallery.classList.toggle("is-paused", paused);
  toggle.setAttribute("aria-pressed", String(paused));
  toggle.textContent = paused ? resumeLabel : pauseLabel;
};
```

```css
.brands-gallery.is-paused {
  animation-play-state: paused;
}
```

## 4. Utilidad para contenido visible solo a tecnologías asistivas — Completado

**Prioridad: media**

**Entregado:** `.visually-hidden` en `assets/css/utility.css` y guía de uso en
[`docs/visually-hidden.md`](../visually-hidden.md).

Añadir una utilidad semánticamente neutra, por ejemplo `.visually-hidden`,
para conservar contenido en el árbol de accesibilidad sin mostrarlo en la
interfaz. Es necesaria para patrones como una única lista semántica que
describe una galería o marquesina visual cuyos elementos repetidos se marcan
con `aria-hidden="true"`.

### Contrato

- La utilidad oculta visualmente el contenido sin eliminarlo del árbol de
  accesibilidad ni impedir que un enlace o control contenido reciba foco.
- No debe depender de colores, puntos de quiebre ni estructura de un sitio
  consumidor.
- Debe documentar que no se usa para ocultar controles interactivos de forma
  permanente; para contenido decorativo se debe usar `aria-hidden="true"` o
  `alt=""` según corresponda.

### Criterios de aceptación

- El contenido con la utilidad se anuncia mediante lectores de pantalla.
- Los controles enfocables dentro de la utilidad se revelan o se documentan
  mediante un patrón específico que evite foco invisible.
- Se incluye un ejemplo de una lista semántica equivalente a una galería visual
  duplicada.

### Referencia provisional aplicada en portafolio.camilo-granados.com

Mientras `ui-core` no ofrezca la utilidad, el sitio incorpora esta regla local
en `assets/css/main.css`. El módulo puede adoptarla como base, revisando la
convención de nombres y cualquier interacción con sus utilidades existentes.

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

La marquesina local conserva una sola lista para tecnologías asistivas y marca
la representación animada, que duplica los logos para lograr un desplazamiento
continuo, como decorativa:

```html
<ul class="visually-hidden">
  <li>Marca A</li>
  <li>Marca B</li>
</ul>

<div class="marquee" aria-hidden="true">
  <div class="brands-gallery">
    <img src="marca-a.svg" alt="">
    <img src="marca-b.svg" alt="">
    <!-- Repeticiones visuales para la animación. -->
    <img src="marca-a.svg" alt="">
    <img src="marca-b.svg" alt="">
  </div>
</div>
```

El contenido de la lista y las rutas de los medios siguen siendo datos
editoriales del consumidor; `ui-core` solo aporta el patrón de ocultación.
