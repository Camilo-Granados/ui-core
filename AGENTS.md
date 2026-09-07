# Guía de contribución para agentes

## Propósito

Este repositorio es un módulo de Hugo que entrega los estilos base del portafolio y sitio público personal de Camilo. Las decisiones de implementación deben favorecer claridad, mantenimiento y una carga inicial rápida.

## Principios de implementación

- Escribe marcado en HTML directo mediante los layouts y parciales de Hugo.
- Escribe estilos en CSS directo dentro de `assets/css/`.
- No agregues paquetes, frameworks de CSS o JavaScript, librerías de componentes, preprocesadores ni herramientas de compilación internas.
- Evita JavaScript salvo que sea imprescindible para una interacción concreta. Si se requiere, usa JavaScript nativo, pequeño y diferido.
- No introduzcas dependencias externas que bloqueen el renderizado, como fuentes, hojas de estilo o scripts remotos, sin una razón explícita y documentada.

## Rendimiento

- Prioriza HTML semántico, CSS ligero y el menor número posible de recursos.
- Reutiliza tokens, utilidades y patrones existentes antes de crear reglas nuevas.
- Evita selectores costosos, duplicación de CSS, animaciones innecesarias y recursos que no se usen en todas las páginas.
- Las imágenes e ilustraciones deben ser apropiadas para web y cargarse de forma diferida cuando no formen parte del contenido visible inicialmente.
- Conserva una experiencia correcta sin JavaScript.

## Estructura del módulo

- `assets/css/`: estilos base organizados por responsabilidad.
- `layouts/partials/`: HTML reutilizable, incluyendo iconos e ilustraciones.
- `docs/`: documentación funcional, técnica y decisiones que se generen durante el proyecto.

## Documentación

- Registra en `docs/` las decisiones que afecten la arquitectura, el sistema visual, accesibilidad o rendimiento.
- Mantén la documentación breve, fechada cuando corresponda y alineada con el código vigente.

## Registro de cambios

`changelog.md` es el registro de versiones para los sitios personales que consumen este módulo y para los agentes que los mantienen. No requiere la formalidad de un producto público, pero debe permitir saber qué cambió y si un sitio necesita actuar antes de actualizar.

- Añade cada versión al inicio con el formato `## vX.Y.Z`.
- Registra solo cambios entregados en esa versión. Mantén una línea por cambio observable para un consumidor; agrupa únicamente archivos que formen una sola entrega, como una documentación relacionada.
- Inicia cada línea con una de estas categorías: `Add` para una capacidad nueva, `Change` para un comportamiento o valor modificado, `Fix` para una corrección y `Remove` para algo retirado.
- Nombra con precisión la clase, token, parcial o ruta afectada entre comillas invertidas. Indica el efecto o propósito cuando el nombre no lo explique por sí solo.
- Si un sitio consumidor requiere modificar su código para actualizar, añade `(breaking)` y explica brevemente qué debe hacer. Si no requiere cambios, no añadas esa marca.
- Evita descripciones vagas como “actualizaciones”, mensajes de commit y detalles internos que no cambien el uso, el resultado visual, la accesibilidad, el rendimiento o la documentación.

Usa este patrón:

```md
## vX.Y.Z
- Add: `ruta/o-identificador` para [propósito].
- Change: `identificador` de [valor anterior] a [valor nuevo] para [efecto].
- Fix: [problema corregido] en `ruta/o-identificador`.
- Remove (breaking): `ruta/o-identificador`; los sitios consumidores deben [acción de migración].
```

Para este módulo, incrementa el parche (`v0.5.3` → `v0.5.4`) en correcciones, documentación y adiciones pequeñas compatibles. Incrementa el número menor (`v0.5.x` → `v0.6.0`) cuando cambie el comportamiento visual o de uso de varios sitios, o cuando una actualización exija revisar su integración. No es necesario publicar una versión mayor mientras el módulo permanezca en `v0.x` y sea de uso personal.

## Antes de finalizar un cambio

- Comprueba que el HTML sea semántico y accesible.
- Comprueba que el CSS no incorpore dependencias ni aumente trabajo innecesario del navegador.
- Verifica el formato de los archivos modificados y, cuando aplique, ejecuta la validación de Hugo disponible en el proyecto.
