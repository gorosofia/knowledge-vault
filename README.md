# Knowledge Vault

Este repositorio es el vault de Obsidian con todas las notas de conocimiento personal.
Sirve como **base de conocimiento compartida** entre el usuario y los agentes de IA
que lo consultan via MCP.

---

## Ciclo de vida del conocimiento

```
CAPTURAR → ORGANIZAR → INDEXAR → CONSULTAR
```

### 1. Capturar

Todo lo que ves en web, X/Twitter, repos de GitHub, newsletters... lo guardas con
**Obsidian Clipper** directamente en `Clippings/`. Sin pensar, sin clasificar, todo ahí.

### 2. Organizar

En una sesión de revisión (diaria o semanal), clasificas lo que hay en `Clippings/`
y `Inbox/` según esta tabla:

| Carpeta | Cuándo usar | Ejemplo |
|---------|-------------|---------|
| **`Inbox/`** | Notas rápidas que necesitas procesar, ideas sueltas, borradores | "Posible arquitectura para agente de empleo" |
| **`Projects/`** | Cosas con **objetivo concreto y fecha de fin**. Una subcarpeta por proyecto | `Projects/job-search-agent/`, `Projects/web-personal/` |
| **`Areas/`** | Responsabilidades continuas que **no tienen fin**. Una subcarpeta por área | `Areas/salud/`, `Areas/finanzas/`, `Areas/carrera/` |
| **`Resources/`** | Referencia temática que **no pertenece a un proyecto concreto** | `Resources/agents/`, `Resources/ats/`, `Resources/deepseek/` |
| **`Clippings/`** | **Buffer temporal** de capturas web. Plano, sin subcarpetas | Artículos, tuits, repos que te parecen interesantes |

**Regla práctica**: pregúntate "si este proyecto no existiera, ¿seguiría queriendo tener esto guardado?"

- Sí → `Resources/<tema>/`
- No → `Projects/<proyecto>/`

> **Importante**: `Clippings/` es plano (sin subcarpetas). Las subcarpetas solo existen en
> `Projects/`, `Areas/` y `Resources/`, porque ahí el conocimiento ya está clasificado.

### 3. Indexar

Ejecutas el indexador del knowledge-engine:

```bash
cd knowledge-engine
source venv/bin/activate
python build_index.py
```

Esto hace:

```
1. Lee todos los archivos .md / .txt / .pdf de knowledge-vault/ (TODAS las carpetas)
2. Divide cada archivo en fragmentos de ~1000 caracteres (chunks)
3. Cada fragmento → Ollama → embedding (vector de 768 números)
4. Guarda todo en ChromaDB: { vector, texto, fuente, posición }
```

**Lo que NO hace la indexación:**
- No modifica tus archivos .md originales
- No entiende el significado (solo posición geométrica en el espacio vectorial)
- No razona sobre la información (eso lo hace el LLM después)

**Lo que SÍ consigue:**
- Búsqueda por **significado semántico**, no solo por palabras clave
- Dos textos con significado parecido tienen vectores cercanos, aunque usen palabras distintas
- Ejemplo: "agentes autónomos" y "sistemas multi-agente" estarán cerca; "receta de paella" lejos

Puedes indexar tantas veces como quieras. Es idempotente: si el contenido no cambió,
los embeddings son los mismos.

### 4. Consultar

El vault se consume a través del **servidor MCP** (`knowledge-engine/mcp_server.py`).

Un agente o script puede preguntar:

```
search_knowledge("¿cómo se estructura un agente de búsqueda de empleo?")
```

El flujo es:

```
Query → Ollama embedding → ChromaDB top-8 fragmentos → DeepSeek genera respuesta
                                                              ↓
                                                    Respuesta con citas del vault
```

**Ejemplos de consumo:**

- `python ask.py "mi pregunta"` — consulta directa desde terminal
- `python mcp_server.py` — expone el vault como herramienta MCP para agentes
- Desde `agent-engineering-lab/` — los ejercicios usan el MCP para aprender a construir agentes
- Desde proyectos nuevos — cualquier agente puede conectarse via MCPClient

> No necesitas mover notas entre carpetas para que sean visibles. El indexador indexa
> **todo** `knowledge-vault/` independientemente de la carpeta. La organización es para
> que tú puedas navegar y mantener el vault, no para el sistema de búsqueda.

---

## Estructura

```
knowledge-vault/
├── Inbox/         ← Notas sin procesar, ideas sueltas, borradores
├── Clippings/     ← Capturas del Obsidian Clipper (buffer temporal, plano)
├── Projects/      ← Conocimiento específico de proyectos (subcarpetas)
├── Areas/         ← Conocimiento de áreas de responsabilidad (subcarpetas)
├── Resources/     ← Referencia general por temas (subcarpetas)
└── README.md      ← Este archivo
```

---

## Integración con agentes

Cuando un agente (o Kilo) necesita tu conocimiento para un proyecto:

```
1. build_index.py indexa el vault completo
2. mcp_server.py expone search_knowledge via MCP stdio
3. El agente se conecta al MCP y pregunta con lenguaje natural
4. El sistema busca fragmentos relevantes y genera respuesta contextualizada
```

El vault es tu **memoria compartida**. Cuanta más información tenga, mejores decisiones
tomarán los agentes. Pero la **decisión final siempre es tuya**: tú defines el proyecto,
las prioridades, las APIs a usar. Los agentes ejecutan, tú diriges.
