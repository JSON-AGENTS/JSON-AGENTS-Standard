# Agents.JSON

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](./CHANGELOG.md)
[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](./LICENSE)
[![JSON Schema](https://img.shields.io/badge/JSON%20Schema-2020--12-purple.svg)](https://json-schema.org/draft/2020-12/json-schema-core.html)
[![Standard](https://img.shields.io/badge/standard-RFC%208259-orange.svg)](https://datatracker.ietf.org/doc/html/rfc8259)
[![Media Type](https://img.shields.io/badge/media%20type-application%2Fagents%2Bjson-teal.svg)](#)
[![Status](https://img.shields.io/badge/status-stable-brightgreen.svg)](./agents-json.md)

> **A Universal JSON Specification for Portable AI Agents**

---

### 🌐 Overview

**Agents.JSON** defines an open, JSON-native specification for describing AI agents, their capabilities, tools, runtimes, and governance in a single portable manifest.

It allows frameworks, SDKs, and orchestrators to **interoperate seamlessly** — sharing agent definitions that are human-readable, machine-validated, and future-proof.

Agents.JSON is based entirely on established JSON standards and validated through JSON Schema.

---

### 🧩 Core Principles

| Principle | Description |
|------------|--------------|
| **JSON-Native** | Derived from RFC 8259, ECMA-404, and ISO 21778. |
| **Schema-Validated** | Enforced through JSON Schema 2020-12. |
| **Profile-Based** | Modular profiles for `core`, `exec`, `gov`, and `graph`. |
| **Governance-Aware** | Security, policies, and observability included by design. |
| **Extensible** | `extensions` and `x-*` namespaces for safe innovation. |
| **Framework-Neutral** | Compatible with any agent runtime or framework. |

---

### 📘 Specification

- **Specification Document:** [`agents-json.md`](./agents-json.md)  
- **Canonical JSON Schema:** [`schema/agents-json.json`](./schema/agents-json.json)

---

### 📂 Repository Layout

```bash
/
├── README.md
├── agents-json.md
├── schema/
│   ├── agents-json.json
│   ├── message-envelope.json
│   └── capabilities/
│       ├── summarization.json
│       ├── routing.json
│       └── retrieval.json
├── examples/
│   ├── core.json
│   ├── core-exec.json
│   ├── core-exec-gov.json
│   └── core-exec-gov-graph.json
├── registry/
│   ├── capabilities.json
│   ├── tool-types.json
│   └── profiles.json
└── docs/
    ├── index.md
    ├── implementers-guide.md
    ├── mapping-frameworks.md
    └── extensions.md
````

---

### 🔗 Specification Family

| Profile   | Description                                          |
| --------- | ---------------------------------------------------- |
| **Core**  | Agent identity, tools, capabilities, and context.    |
| **Exec**  | Runtime metadata, language, entrypoint, environment. |
| **Gov**   | Security, policy, and observability.                 |
| **Graph** | Multi-agent topology and message envelope.           |

---

### 🧠 Example

```json
{
  "manifest_version": "1.0",
  "profiles": ["core", "exec", "gov", "graph"],
  "agent": {
    "id": "ajson://agents.json/examples/router-hub",
    "name": "Router Hub"
  },
  "runtime": { "type": "node", "entrypoint": "dist/router.js" },
  "security": { "sandbox": "process" },
  "graph": {
    "nodes": [
      { "id": "router", "ref": "ajson://agents.json/examples/router-hub" },
      { "id": "faq", "ref": "ajson://agents.json/examples/faq" }
    ],
    "edges": [
      { "from": "router", "to": "faq", "condition": "message.intent == 'faq'" }
    ]
  }
}
```

---

### ⚖️ License

Agents.JSON is released under the **Apache 2.0 License**.
See [`LICENSE`](./LICENSE) for details.

---

### 🧭 Contributing

Contributions and proposals are welcome via pull requests or discussions.
See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for guidance.

---

### 🧱 Versioning

Version identifiers follow [Semantic Versioning 2.0](https://semver.org/).
The default branch represents the **latest stable version** of the specification.

---

© 2025 Agents.JSON. All rights reserved.
