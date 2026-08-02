# Context7 MCP

Usa Context7 para consultar documentación actualizada y específica de versión cuando una tarea involucre librerías, frameworks, SDK, API, CLI o servicios externos.

## Flujo obligatorio

1. Identifica la librería y su versión revisando los archivos del proyecto, por ejemplo:
   - `package.json`
   - `pom.xml`
   - `build.gradle`
   - `requirements.txt`
   - `pyproject.toml`
   - Archivos de configuración equivalentes.

2. Si ya se proporcionó un ID de Context7 con formato:

   ```text
   /organizacion/proyecto
   /organizacion/proyecto/version
   ```

   úsalo directamente.

3. Si no existe un ID conocido, llama primero a `resolve-library-id` usando:
   - El nombre oficial de la librería.
   - Una descripción concreta de la tarea.

4. Selecciona el resultado que mejor coincida con:
   - Nombre oficial.
   - Versión utilizada por el proyecto.
   - Reputación de la fuente.
   - Cobertura de documentación.
   - Relevancia para la tarea.

5. Llama a `query-docs` con el ID seleccionado y una consulta específica.

6. Separa en distintas consultas los temas independientes. No combines, por ejemplo, autenticación, caché y routing en una sola solicitud.

7. Implementa la solución únicamente con APIs, configuraciones y ejemplos respaldados por la documentación recuperada.

## Reglas

- Prioriza siempre la versión utilizada por el proyecto.
- No inventes métodos, propiedades, anotaciones, parámetros o configuraciones.
- No uses información recordada cuando Context7 pueda verificarla.
- No envíes contraseñas, tokens, API keys, datos personales ni código confidencial.
- Realiza como máximo tres llamadas de resolución y tres consultas de documentación por tarea.
- No uses Context7 para lógica de negocio, refactorizaciones simples o conceptos generales que no dependan de documentación externa.
- Si la documentación es insuficiente, ambigua o no corresponde a la versión utilizada, indícalo claramente en lugar de asumir.
- Al finalizar, menciona brevemente la librería y la versión de documentación consultadas.
