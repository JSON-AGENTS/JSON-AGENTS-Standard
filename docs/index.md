# Agents.JSON Documentation Index

> The central reference for understanding, implementing, and extending the **Agents.JSON** specification.

---

## 📖 Overview

**Agents.JSON** is a vendor-neutral, JSON-native format for defining **AI agents** in a portable, interoperable way.

It allows agents, tools, and orchestrators to communicate using a shared schema that captures:

- **Identity & Capabilities**
- **Runtime & Execution Metadata**
- **Governance & Observability**
- **Graph-based Compositions**

Each layer is defined through modular **Profiles**, ensuring systems can implement only what they need.

---

## 🧱 Specification Family

| Document | Purpose |
|-----------|----------|
| [`agents-json.md`](../agents-json.md) | Normative specification (definition, fields, and rules). |
| [`schema/agents-json.json`](../schema/agents-json.json) | Canonical JSON Schema for validation. |
| [`/schema/`](../schema) | Component schemas (envelope, capabilities). |
| [`/examples/`](../examples) | Example manifests for all profile combinations. |
| [`/registry/`](../registry) | Canonical registries of capabilities, profiles, and tool types. |
| [`/docs/`](../docs) | Human-readable documentation and implementer guides. |

---

## 📚 Core Concepts

| Concept | Description |
|----------|--------------|
| **Agent** | A software entity defined by a JSON manifest containing identity, capabilities, and runtime context. |
| **Capability** | A modular unit of functionality, such as summarization or routing, defined through reusable JSON Schemas. |
| **Tool** | An external or internal callable function, API, or plugin available to the agent. |
| **Profile** | A modular extension defining new required or optional fields (e.g., `core`, `exec`, `gov`, `graph`). |
| **Graph** | A declarative topology describing how agents connect and exchange messages. |
| **Envelope** | The canonical message structure for communication between agents. |

---

## 🧩 Profiles at a Glance

| Profile | Focus | Example Use |
|----------|--------|-------------|
| **Core** | Identity, capabilities, and modalities | Minimal agent definition |
| **Exec** | Runtime metadata, entrypoint, resources | Containerized agents |
| **Gov** | Policies, observability, security | Enterprise or compliance-sensitive workloads |
| **Graph** | Multi-agent routing and topologies | Compositional orchestration environments |

---

## 🔗 Related Standards

Agents.JSON is **derived entirely from JSON family standards**:

- [RFC 8259 — The JSON Data Interchange Syntax](https://datatracker.ietf.org/doc/html/rfc8259)
- [ECMA-404 — The JSON Data Interchange Format](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)
- [JSON Schema 2020-12](https://json-schema.org/draft/2020-12/json-schema-core.html)
- [ISO/IEC 21778:2017 — Information technology — The JSON data interchange syntax](https://www.iso.org/standard/71616.html)

Agents.JSON follows these standards faithfully, adding domain semantics for AI agent interoperability.

---

## 🧭 Implementation Resources

- [Implementer’s Guide](./implementers-guide.md)  
  → How to validate, parse, and integrate manifests.

- [Framework Mapping](./mapping-frameworks.md)  
  → How LangChain, AutoGen, OpenAI Agents, and others map to Agents.JSON.

- [Extensions Guide](./extensions.md)  
  → How to safely define and register your own extensions using the `x-*` namespace.

---

## ⚖️ License

Released under the **Apache 2.0 License**.  
See [`LICENSE`](../LICENSE) for full text.

---

© 2025 Agents.JSON Project. All rights reserved.
