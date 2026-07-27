# Primera Nota de Prueba

Gorosophia es un sistema de conocimiento personal basado en:

- **Obsidian** para la edición de notas en Markdown.
- **Ollama** con el modelo `nomic-embed-text` para generar embeddings localmente.
- **ChromaDB** como base de datos vectorial para almacenar y recuperar fragmentos.
- **DeepSeek V4 Flash** como LLM para responder preguntas con contexto RAG.
- **MCP** como protocolo para exponer herramientas a agentes externos.

## Componentes

1. `knowledge-vault` — vault de Obsidian con estructura PARA.
2. `knowledge-engine` — motor que indexa, consulta y expone MCP.

## Flujo de trabajo

1. Escribir notas en Obsidian (`knowledge-vault`).
2. Indexar con `python build_index.py` desde `knowledge-engine`.
3. Consultar con `python ask.py "pregunta"` o vía MCP.
4. Respaldar con Git en ambos repositorios.
