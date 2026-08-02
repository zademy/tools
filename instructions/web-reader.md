# Web Reader MCP

Usa `web-reader` para leer y extraer el contenido completo de una URL concreta.

## Herramienta disponible

- `webReader`: obtiene el título, contenido principal, metadatos y enlaces de una página web.

## Cuándo usarlo

- Cuando el usuario proporcione una URL.
- Después de localizar una fuente relevante con `web-search-prime`.
- Para consultar documentación, notas de versión, artículos técnicos, guías, README públicos o páginas de referencia.
- Cuando sea necesario verificar el contenido real de una página y no solamente un resumen de búsqueda.

## Flujo recomendado

1. Confirma que la URL corresponde a la fuente relevante.
2. Lee la página con `webReader`.
3. Identifica el contenido principal, la fecha, la versión y las advertencias aplicables.
4. Extrae únicamente las secciones relacionadas con la tarea.
5. Sigue enlaces internos solo cuando sean necesarios para completar la respuesta.
6. Resume los hallazgos sin alterar su significado.
7. Si la página no contiene la respuesta, usa `web-search-prime` para localizar otra fuente.

## Reglas

- No inventes contenido cuando la página esté vacía, bloqueada o no sea accesible.
- No confundas metadatos, menús o enlaces de navegación con el contenido principal.
- No sigas todos los enlaces de una página sin una razón concreta.
- Prioriza páginas oficiales y documentación correspondiente a la versión utilizada.
- Distingue entre texto explícito de la fuente e inferencias propias.
- No envíes credenciales, tokens ni información privada dentro de la URL.
- No uses `web-reader` para explorar la estructura o archivos de un repositorio; utiliza `zread`.
- No uses `web-reader` para descubrir fuentes cuando todavía no se conoce una URL; utiliza `web-search-prime`.
