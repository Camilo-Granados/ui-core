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

## Antes de finalizar un cambio

- Comprueba que el HTML sea semántico y accesible.
- Comprueba que el CSS no incorpore dependencias ni aumente trabajo innecesario del navegador.
- Verifica el formato de los archivos modificados y, cuando aplique, ejecuta la validación de Hugo disponible en el proyecto.
