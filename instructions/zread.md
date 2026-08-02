# Zread MCP

Usa `zread` para investigar repositorios públicos de GitHub y comprender su documentación, estructura y código fuente.

## Herramientas disponibles

- `search_doc`: busca documentación, código, comentarios, noticias, issues, commits, PRs y colaboradores relacionados con un repositorio.
- `get_repo_structure`: obtiene la estructura de directorios y archivos de un repositorio o subdirectorio.
- `read_file`: lee el contenido completo de un archivo específico.

## Cuándo usarlo

- Para comprender una librería o proyecto de código abierto alojado en GitHub.
- Para localizar la implementación de una clase, función, módulo o característica.
- Para revisar la arquitectura y organización de un repositorio.
- Para investigar issues, cambios recientes o decisiones documentadas del proyecto.
- Para leer archivos concretos antes de proponer una integración, corrección o refactorización.

## Flujo recomendado

1. Identifica el repositorio con formato `owner/repo`.
2. Usa `search_doc` para obtener una visión general o localizar conceptos relevantes.
3. Usa `get_repo_structure` para conocer la organización del proyecto y encontrar rutas exactas.
4. Usa `read_file` únicamente cuando exista una ruta concreta y relevante.
5. Relaciona los hallazgos con archivos, módulos y símbolos específicos.
6. Para una investigación profunda, repite el ciclo de búsqueda, estructura y lectura solo en las áreas necesarias.
7. Explica qué proviene del repositorio y qué corresponde a una inferencia técnica.

## Reglas

- Trabaja únicamente con repositorios públicos compatibles con Zread.
- No inventes rutas, archivos, clases, funciones ni comportamiento.
- No leas archivos grandes sin haber reducido primero el alcance.
- No uses `read_file` sin una ruta suficientemente precisa.
- Prioriza archivos fuente, documentación oficial del repositorio, pruebas y configuración.
- Cuando existan varias implementaciones, identifica cuál corresponde a la versión o rama analizada.
- No uses Zread para archivos locales del proyecto actual.
- No uses Zread para páginas web generales; utiliza `web-reader`.
- No uses Zread para búsquedas generales de Internet; utiliza `web-search-prime`.
- No incluyas tokens, credenciales ni información privada en las consultas.
