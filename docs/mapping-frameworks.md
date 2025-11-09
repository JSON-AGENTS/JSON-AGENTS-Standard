# Framework & Format Mapping

> How existing AI agent frameworks and formats can interoperate with **Agents.JSON**.

---

## 🎯 Purpose

This document provides a **crosswalk** between Agents.JSON and other widely used agent frameworks.  
It is intended for developers seeking to **convert**, **import**, or **export** manifests to and from other ecosystems.

Agents.JSON acts as a **unifying layer**, not a competing runtime — its goal is to describe any agent in a way that is portable and self-contained.

---

## 🧭 1. Alignment Philosophy

| Principle | Description |
|------------|--------------|
| **Declarative over imperative** | Agents.JSON is a description of what an agent *is*, not how it runs. |
| **Stable identity** | Every agent has a globally unique `id`. |
| **Capability modularity** | Each capability has its own schema. |
| **Runtime neutrality** | Manifest does not assume Python, Node, or Go — it's runtime-agnostic. |
| **Interoperable serialization** | Converts easily to and from framework manifests using JSON or YAML. |
| **Bidirectional conversion** | Supports both **Framework → Agents.JSON** (import/ingest) and **Agents.JSON → Framework** (export/deploy). |

---

## 🔄 1.1 Conversion Patterns

### Framework → Agents.JSON (Import/Ingest)

Convert existing framework-specific agent definitions into portable Agents.JSON manifests:

- **Extract** agent identity, capabilities, and tool definitions from framework-native formats
- **Map** framework-specific fields to Agents.JSON standard properties
- **Normalize** runtime configurations to `runtime` profile
- **Preserve** framework-specific metadata in `extensions` for round-trip fidelity

**Use cases:** Agent registry, cross-framework discovery, governance audits, migration planning

### Agents.JSON → Framework (Export/Deploy)

Generate framework-specific agent implementations from Agents.JSON manifests:

- **Translate** Agents.JSON `tools` array to framework tool registries
- **Generate** framework boilerplate (e.g., LangChain chains, AutoGen agent configs)
- **Map** `runtime.entrypoint` to framework initialization code
- **Apply** capability constraints to framework execution patterns

**Use cases:** Multi-framework deployment, runtime generation, agent templating, infrastructure-as-code

---

## 🔌 2. Mapping Summary

| Framework / Format | Equivalent to | Mapping Method |
|--------------------|----------------|----------------|
| **LangChain (Python / JS)** | Chains / Tools / Agents | Convert chains to `tools`; define each tool’s input/output schema; runtime = `python` |
| **OpenAI Agents** | `manifest.json` | Rename fields: `name_for_model` → `agent.name`, `schema_version` → `manifest_version` |
| **AutoGen** | Agent graph | Direct mapping from `graph.nodes` / `graph.edges` |
| **Hugging Face Transformers Agent** | Tool registry | Tools map 1:1; model = `runtime.entrypoint` |
| **MCP (Model Context Protocol)** | Tool metadata | Each `tool` can embed MCP descriptors as `type: "mcp"` |
| **CrewAI / ReAct** | Capability layer | Each ReAct step = `capability` or tool call |
| **LLM Gateway YAMLs (Ollama, vLLM)** | Runtime configuration | Translate environment variables to `runtime.resources` |
| **Anthropic / Claude Tool Use** | Function definitions | Map directly to `tools[].input_schema` and `output_schema` |
| **Vercel AI SDK** | Tool definitions / Multi-step flows | Map `tools` array to Agents.JSON `tools`; streamable UI to `modalities.output` |

---

## ⚙️ 3. LangChain Example Conversion

### LangChain Python

```python
from langchain.agents import initialize_agent, load_tools
tools = load_tools(["serpapi", "llm-math"], llm=llm)
agent = initialize_agent(tools, llm, agent_type="zero-shot-react-description")
````

### → Agents.JSON Equivalent

```json
{
  "manifest_version": "1.0",
  "profiles": ["core", "exec"],
  "agent": {
    "id": "ajson://example/langchain-agent",
    "name": "LangChain-Compatible Agent"
  },
  "capabilities": [
    { "id": "qa", "description": "Answer queries via search and reasoning." }
  ],
  "tools": [
    { "id": "tool://serpapi", "type": "http", "endpoint": "https://serpapi.com" },
    { "id": "tool://llm-math", "type": "function", "description": "Evaluate mathematical expressions." }
  ],
  "runtime": {
    "type": "python",
    "entrypoint": "agent.py",
    "resources": { "cpu_cores_min": 1, "memory_mb_min": 512 }
  }
}
```

---

## 💬 4. OpenAI Manifest Mapping

| OpenAI Manifest Field   | Agents.JSON Equivalent                   |
| ----------------------- | ---------------------------------------- |
| `schema_version`        | `manifest_version`                       |
| `name_for_human`        | `agent.name`                             |
| `name_for_model`        | `agent.id` or `agent.alias`              |
| `description_for_model` | `agent.description`                      |
| `api.url`               | `tools[].endpoint`                       |
| `api.parameters`        | `tools[].input_schema`                   |
| `auth.type`             | `tools[].auth.method`                    |
| `contact_email`         | `agent.contact.email` (custom extension) |

---

## 🧩 5. MCP (Model Context Protocol) Mapping

Agents.JSON tools can embed MCP-like metadata:

```json
{
  "id": "tool://mcp/summarizer",
  "type": "mcp",
  "description": "Summarization tool via MCP server.",
  "endpoint": "https://mcp.server.local",
  "metadata": {
    "provider": "local",
    "protocol": "MCP/1.0"
  }
}
```

This allows one-to-one interoperability between Agents.JSON manifests and any MCP server definitions.

---

## 🧠 6. AutoGen / Multi-Agent Graphs

AutoGen and similar frameworks define **agent teams** or **conversation graphs**.
These map directly to the Agents.JSON `graph` profile:

```json
{
  "graph": {
    "nodes": [
      { "id": "planner", "ref": "ajson://example/planner" },
      { "id": "executor", "ref": "ajson://example/executor" }
    ],
    "edges": [
      { "from": "planner", "to": "executor", "condition": "task.assigned == true" }
    ]
  }
}
```

---

## 🔄 7. YAML ↔ JSON Interoperability

For developer friendliness, Agents.JSON can be serialized in **YAML** (e.g., `agents.yaml`) without losing fidelity.

```yaml
manifest_version: "1.0"
profiles: [core, exec]
agent:
  id: ajson://example/yaml-agent
  name: YAML Example Agent
runtime:
  type: node
  entrypoint: dist/index.js
```

All official tooling MUST support both `.json` and `.yaml` input formats.

---

## 🔍 8. Limitations and Mismatches

| Limitation                                                     | Reason                                                                      |
| -------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Some frameworks use imperative runtime setup (e.g., LangGraph) | Agents.JSON is declarative; orchestration logic must be abstracted.         |
| Embedded code or model definitions are discouraged             | Encourages separation of manifest from executable payload.                  |
| Frameworks with opaque plugin systems (e.g., AutoGPT plugins)  | Require manual mapping to `tool` entries until a shared descriptor exists.  |
| Dynamic agent creation                                         | Supported only through external orchestration, not within static manifests. |

---

## 🧭 9. Conversion Utilities

Community toolkits (planned):

* **`ajson convert --from=openai --to=agents`**
  Convert OpenAI manifests automatically.

* **`ajson validate`**
  Validate against Agents.JSON schema.

* **`ajson inspect`**
  Pretty-print manifest structure with color-coded sections.

---

## ⚖️ 10. Implementation Notes

When bridging frameworks:

* Preserve all UUIDs and unique IDs as stable references.
* Prefer canonical schema URIs for validation.
* Maintain `manifest_version` parity across converters.
* Use `x-*` namespaces for fields that have no direct counterpart.

---

© 2025 Agents.JSON Project. All rights reserved.
