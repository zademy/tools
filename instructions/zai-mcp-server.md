# Z.AI Vision MCP Server

Usa `zai-mcp-server` para analizar imágenes, capturas de pantalla, diagramas, gráficas y videos mediante las capacidades visuales de Z.AI.

## Herramientas disponibles

- `ui_to_artifact`: convierte una captura de interfaz en código, especificaciones, prompts o descripciones.
- `extract_text_from_screenshot`: extrae texto, código, documentación, logs o contenido de terminal desde una captura.
- `diagnose_error_screenshot`: analiza una captura de error y propone causas y acciones correctivas.
- `understand_technical_diagram`: interpreta diagramas de arquitectura, flujo, UML, entidad-relación y sistemas.
- `analyze_data_visualization`: analiza gráficas, dashboards, métricas, tendencias y anomalías.
- `ui_diff_check`: compara dos capturas de interfaz e identifica diferencias visuales o de implementación.
- `image_analysis`: realiza análisis visual general cuando ninguna herramienta especializada sea adecuada.
- `video_analysis`: analiza videos locales o remotos para describir escenas, momentos y entidades.

## Selección de herramienta

- Para reproducir una interfaz o generar código desde una captura, usa `ui_to_artifact`.
- Para obtener texto exacto de una captura, usa `extract_text_from_screenshot`.
- Para errores visibles en IDE, terminal o navegador, usa `diagnose_error_screenshot`.
- Para diagramas técnicos, usa `understand_technical_diagram`.
- Para gráficas y dashboards, usa `analyze_data_visualization`.
- Para comparar diseño esperado contra implementación, usa `ui_diff_check`.
- Para imágenes que no encajen en los casos anteriores, usa `image_analysis`.
- Para videos, usa `video_analysis`.

## Flujo recomendado

1. Verifica que el archivo exista y proporciona su ruta local o URL.
2. Selecciona la herramienta más específica para la tarea.
3. Indica qué debe identificarse y el formato esperado de la respuesta.
4. Separa observaciones visibles de interpretaciones o hipótesis.
5. Cuando se analice una interfaz, identifica estructura, componentes, estados, tipografía, espaciado y comportamiento observable.
6. Cuando se diagnostique un error, relaciona el texto visible con el contexto técnico disponible.
7. Usa `image_analysis` solo como alternativa general.

## Reglas

- En OpenCode, referencia preferentemente la ruta del archivo; pegar una imagen directamente puede evitar que el cliente invoque este MCP.
- No inventes texto que no sea legible.
- Indica cuando una conclusión sea incierta o dependa de información no visible.
- No afirmes que una interfaz funciona de cierta manera basándote únicamente en una captura estática.
- Para `ui_diff_check`, identifica claramente la imagen de referencia y la imagen candidata.
- Para videos, utiliza formatos compatibles y respeta el límite admitido por el servidor.
- No expongas secretos, datos personales ni información sensible presente en imágenes o videos.
- No uses herramientas visuales para información que pueda verificarse directamente en el código o documentación.
