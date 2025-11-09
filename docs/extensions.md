# Extensions Guide

> How to safely extend **Agents.JSON** using the `x-*` namespace and structured extension definitions.

---

## 🌐 1. Purpose

Agents.JSON is designed to evolve.  
To prevent schema fragmentation while allowing innovation, the specification defines an **extensions mechanism** that lets implementers add new fields, namespaces, and behaviors without breaking compatibility.

---

## 🧭 2. Principles

| Principle | Description |
|------------|--------------|
| **Namespaced** | All custom additions MUST begin with `x-`. |
| **Non-breaking** | Extensions MUST NOT redefine existing fields. |
| **Self-contained** | Each extension SHOULD declare its structure and type. |
| **Discoverable** | Reusable extensions SHOULD be published in `/registry/extensions.json`. |
| **Safe defaults** | Agents or parsers MUST ignore unknown `x-*` keys gracefully. |

---

## 🧱 3. Basic Usage

Extensions can appear **anywhere** in a manifest — top-level, nested, or within sub-objects.

```json
{
  "manifest_version": "1.0",
  "profiles": ["core"],
  "agent": {
    "id": "ajson://example/extended-agent",
    "name": "Extended Agent"
  },
  "x-research": {
    "paper_id": "arXiv:2501.0456",
    "hypothesis": "Dynamic agents outperform static chains by 30%"
  }
}
````

Parsers should preserve but ignore unknown `x-*` sections.

---

## 🧩 4. Typed Extensions

To make extensions interoperable, each may include an `$schema` reference:

```json
{
  "x-audit": {
    "$schema": "https://agents.json.org/extensions/audit.json",
    "level": "detailed",
    "include_payloads": false
  }
}
```

Extension schemas are defined independently in `/schema/extensions/`.

---

## 🧠 5. Common Use Cases

| Extension Namespace | Purpose                                                   |
| ------------------- | --------------------------------------------------------- |
| `x-audit`           | Adds custom logging or observability hooks.               |
| `x-memory`          | Defines persistent or episodic memory configuration.      |
| `x-embeddings`      | Declares vectorization or embedding providers.            |
| `x-annotations`     | Stores metadata about datasets, corpora, or prompt types. |
| `x-research`        | Experimental fields for internal benchmarks.              |

---

## 🧰 6. Example: Persistent Memory Extension

```json
{
  "x-memory": {
    "$schema": "https://agents.json.org/extensions/memory.json",
    "provider": "vector-db",
    "engine": "qdrant",
    "capacity": 50000,
    "retention_policy": "LRU"
  }
}
```

This would define additional memory configuration outside the core specification.

---

## 🧾 7. Example: Audit Extension Schema

### `/schema/extensions/audit.json`

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://agents.json.org/schema/extensions/audit.json",
  "title": "Extension: Audit",
  "description": "Custom auditing configuration for Agents.JSON manifests.",
  "type": "object",
  "properties": {
    "level": {
      "type": "string",
      "enum": ["minimal", "detailed", "full"],
      "default": "minimal"
    },
    "include_payloads": {
      "type": "boolean",
      "default": false
    }
  },
  "additionalProperties": false
}
```

---

## 🧩 8. Declaring Extension Registries

All extensions SHOULD be listed in a central registry file:

### `/registry/extensions.json`

```json
{
  "$id": "https://agents.json.org/registry/extensions.json",
  "version": "2025-11-09",
  "extensions": [
    {
      "id": "x-audit",
      "schema": "https://agents.json.org/schema/extensions/audit.json",
      "description": "Adds auditing options to agent manifests."
    },
    {
      "id": "x-memory",
      "schema": "https://agents.json.org/schema/extensions/memory.json",
      "description": "Persistent memory and vector store configuration."
    }
  ]
}
```

---

## 🧮 9. Validation Rules

* Validators MUST treat all `x-*` keys as **optional**.
* Validators MUST NOT raise errors for unrecognized extensions.
* Extensions SHOULD include `$schema` if intended for reuse.
* Schema authors MAY enforce constraints through `$ref` inclusion in their tooling.

---

## 🔒 10. Security Considerations

When adding extensions:

* Avoid embedding secrets or authentication tokens.
* Treat all `x-*` data as untrusted input.
* Implement access control when extensions expose remote endpoints.
* Consider data retention and privacy implications when extending logs or memory.

---

## 🧠 11. Future-Proofing

Future versions of Agents.JSON will support:

* **Extension discovery APIs**
* **Signed extension manifests**
* **Registry synchronization via JSON-LD context**

Until then, extensions remain static, versioned JSON objects validated via `$schema`.

---

© 2025 Agents.JSON Project. All rights reserved.

