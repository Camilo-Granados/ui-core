# Enlaces de salto

La utilidad `.skip-link` presenta un enlace de salto sin imponer su texto,
destino ni estructura. El sitio consumidor debe colocar el enlace antes de
cualquier otro elemento que pueda recibir foco, normalmente al inicio de
`<body>`, y aportar un destino existente.

```go-html-template
<body>
  <a class="skip-link" href="#main-content">Saltar al contenido principal</a>

  <header>…</header>
  <main id="main-content">
    …
  </main>
</body>
```

El enlace permanece fuera de la vista hasta recibir `:focus-visible`, conserva
el indicador de foco global y se sitúa de forma fija para no desplazar el
contenido. Use un texto localizado que describa el destino; por ejemplo,
«Saltar al contenido principal». No oculte el enlace con `display: none`,
`visibility: hidden` ni `aria-hidden`, porque dejaría de estar disponible para
teclado.
