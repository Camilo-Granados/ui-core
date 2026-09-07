# Contenido solo para tecnologías asistivas

La utilidad `.visually-hidden` conserva contenido en el árbol de
accesibilidad sin mostrarlo visualmente. Úsela para información que complemente
una representación visual decorativa, no para ocultar controles interactivos
de forma permanente. Si un enlace o control dentro de la utilidad recibe foco,
la regla se revierte mediante `:focus-within` para evitar un foco invisible.

Por ejemplo, una galería que repite logos para animarse puede mantener una sola
lista semántica para lectores de pantalla y tratar la representación visual
como decorativa:

```html
<ul class="visually-hidden">
  <li>Marca A</li>
  <li>Marca B</li>
</ul>

<div class="brands-gallery" aria-hidden="true">
  <img src="marca-a.svg" alt="">
  <img src="marca-b.svg" alt="">
  <!-- Repeticiones visuales necesarias para la animación. -->
  <img src="marca-a.svg" alt="">
  <img src="marca-b.svg" alt="">
</div>
```

El consumidor aporta los textos y los recursos localizados. Para contenido
puramente decorativo, use `aria-hidden="true"` o `alt=""`, según corresponda,
en vez de `.visually-hidden`.
