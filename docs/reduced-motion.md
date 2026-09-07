# Reducción de movimiento

`assets/css/animations.css` ofrece una política optativa de reducción de
movimiento. El sitio consumidor debe cargar ese archivo cuando use las
duraciones del módulo:

```css
.notice {
  transition: opacity var(--duration-base) ease;
}
```

Con `prefers-reduced-motion: reduce`, `--duration-fast`, `--duration-base` y
`--duration-slow` pasan a `0.01ms`. Las transiciones no esenciales que utilicen
estos tokens dejan de percibirse, mientras que el estado base permanece visible
sin JavaScript.

Las apariciones deben ser una mejora progresiva: defina el contenido visible
por defecto y aplique el estado inicial de opacidad o transformación solo
después de comprobar que la persona no ha solicitado reducción de movimiento.

```css
.has-motion .animate-fade-up {
  opacity: 0;
  transform: translateY(var(--space-s));
  transition: opacity var(--duration-slow) ease,
    transform var(--duration-slow) ease;
}

.has-motion .animate-fade-up.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

Las animaciones continuas, como una marquesina, requieren un control propio de
pausa o desactivación. El consumidor debe proporcionar ese control, actualizar
su nombre y estado accesibles, y pausar la animación del componente; esta
política global no puede conocer su semántica ni sustituirlo.
