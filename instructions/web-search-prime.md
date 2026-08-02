# Web Search Prime MCP

Usa `web-search-prime` para localizar información pública, reciente y verificable en Internet.

## Herramienta disponible

- `webSearchPrime`: realiza búsquedas web y devuelve títulos, URLs, resúmenes y datos del sitio.

## Cuándo usarlo

- Cuando el usuario solicite buscar, investigar, verificar o consultar información actual.
- Para noticias, versiones recientes, cambios de APIs, errores conocidos, disponibilidad de servicios y documentación vigente.
- Cuando todavía no se conoce la URL exacta de la fuente.
- Para encontrar fuentes oficiales antes de implementar una solución técnica.

## Flujo recomendado

1. Convierte la solicitud en una consulta concreta y breve.
2. Incluye nombres oficiales, versiones, mensajes de error o fechas cuando estén disponibles.
3. Prioriza documentación oficial, repositorios oficiales, especificaciones y fuentes primarias.
4. Si los resultados son ambiguos, refina la búsqueda en lugar de asumir.
5. Cuando una afirmación dependa del contenido completo de una página, abre la URL con `web-reader`.
6. Resume únicamente la información respaldada por los resultados encontrados.

## Reglas

- No inventes resultados, URLs, versiones ni características.
- No presentes el resumen de búsqueda como si fuera el contenido completo de una página.
- Para información crítica, compara más de una fuente confiable.
- Distingue claramente entre hechos encontrados e inferencias propias.
- No incluyas API keys, tokens, contraseñas, datos personales ni código confidencial en las consultas.
- No uses esta herramienta para leer el código de un repositorio; utiliza `zread`.
- No uses esta herramienta cuando ya exista una URL concreta que deba leerse; utiliza `web-reader`.
- Evita búsquedas repetidas que no aporten información nueva.
