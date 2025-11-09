Network Working Draft                                       Agents.JSON
Category: Informational                                       9 Nov 2025
Expires: 9 May 2026

# The Agents.JSON Specification

### Abstract

Agents.JSON defines a portable, JSON-native format for describing AI agents,
their capabilities, tools, runtimes, and governance metadata.  
It provides a vendor-neutral and interoperable structure that allows different
frameworks, runtimes, and orchestrators to exchange agent definitions
without loss of meaning.  

The specification extends the family of JSON standards (RFC 8259, ECMA-404,
ISO 21778) with domain-specific semantics for autonomous and assistive
computing systems.

---

### Status of This Memo

This Internet-Draft is submitted for public review and comment.
Distribution is unlimited.  
Comments are solicited via public repositories and discussion forums.

---

## 1. Introduction

AI agents have become foundational components of modern systems.
However, frameworks and vendors currently define incompatible
manifest formats.  
Agents.JSON introduces a **common language** that is:
- JSON-based
- Schema-validated
- Extensible through the `x-*` namespace
- Backward-compatible with existing data interchange standards

---

## 2. Terminology

| Term | Meaning |
|------|----------|
| **Agent** | A computational entity capable of receiving input, performing reasoning or actions, and producing output. |
| **Capability** | A declarative description of a function or skill the agent can perform. |
| **Tool** | A callable external or internal resource invoked by an agent. |
| **Profile** | A modular section of the specification defining required fields for a functional area (`core`, `exec`, `gov`, `graph`). |
| **Manifest** | A JSON document conforming to the Agents.JSON schema that defines a single agent or composition. |
| **Extension** | A namespaced addition beginning with `x-`, providing optional experimental or organization-specific data. |

---

## 3. Design Goals

- **Portability:** manifests move between runtimes without transformation.  
- **Composability:** agents reference each other using URIs.  
- **Extensibility:** schema-level modularity through profiles and extensions.  
- **Governance:** standardized metadata for security and observability.  
- **Interoperability:** direct mapping to formats like OpenAI manifests, LangChain, and AutoGen.

---

## 4. JSON Schema Conformance

All Agents.JSON documents MUST conform to [JSON Schema 2020-12](https://json-schema.org).
Implementations MUST reject invalid manifests.

The canonical schema is located at:

```

[https://agents.json.org/schema/agents-json.json](https://agents.json.org/schema/agents-json.json)

````

---

## 5. Core Structure

```json
{
  "manifest_version": "1.0",
  "profiles": ["core", "exec", "gov", "graph"],
  "agent": { "id": "ajson://example/router-hub", "name": "Router Hub" },
  "capabilities": [{ "id": "routing" }],
  "runtime": { "type": "node", "entrypoint": "dist/router.js" },
  "graph": {
    "nodes": [{ "id": "router" }, { "id": "faq" }],
    "edges": [{ "from": "router", "to": "faq" }]
  }
}
````

---

## 6. Profiles

| Profile   | Description                                     |
| --------- | ----------------------------------------------- |
| **core**  | Base: identity, capabilities, tools, modalities |
| **exec**  | Runtime: environment, entrypoint, resources     |
| **gov**   | Governance: security, policies, observability   |
| **graph** | Topology: nodes, edges, and message envelopes   |

Profiles are additive; implementations MAY support subsets.

---

## 7. Capabilities and Tools

Capabilities reference schemas such as:

```
https://agents.json.org/schema/capabilities/summarization.json
```

Tools include `type`, `input_schema`, `output_schema`, and optional `auth`.

```json
{
  "id": "tool://summarizer/local-model",
  "type": "function",
  "input_schema": { "type": "string" },
  "output_schema": { "type": "string" }
}
```

---

## 8. Governance

The `gov` profile introduces structured policy and observability.

```json
{
  "security": { "sandbox": "container", "network_zone": "internal" },
  "policies": [
    {
      "id": "deny-external-http",
      "effect": "deny",
      "action": "tool.call",
      "where": "tool.endpoint !~ 'internal.corp'"
    }
  ],
  "observability": {
    "log_level": "info",
    "metrics_enabled": true
  }
}
```

---

## 9. Graph Composition

Agents.JSON defines graphs for orchestrating multi-agent systems:

```json
{
  "graph": {
    "nodes": [
      { "id": "router", "ref": "ajson://router" },
      { "id": "faq", "ref": "ajson://faq" }
    ],
    "edges": [
      { "from": "router", "to": "faq", "condition": "message.intent == 'faq'" }
    ]
  }
}
```

Messages exchanged in the graph MUST conform to
[`/schema/message-envelope.json`](schema/message-envelope.json).

---

## 10. Extensions

Extensions enable experimentation without fragmentation.

```json
{
  "x-memory": {
    "$schema": "https://agents.json.org/schema/extensions/memory.json",
    "provider": "vector-db",
    "engine": "qdrant"
  }
}
```

Registered extensions appear in
[`/registry/extensions.json`](registry/extensions.json).

---

## 11. Security Considerations

* Manifests MUST NOT include credentials in plain text.
* All URIs SHOULD use HTTPS.
* Extensions that add network or storage access MUST specify sandboxing.
* Agents operating in regulated environments SHOULD declare audit retention policies.

---

## 12. IANA Considerations

Media Type:

```
application/agents+json
```

File extension:

```
.agents.json
```

---

## 13. Acknowledgments

The Agents.JSON design draws on lessons from JSON Schema,
OpenAPI, and model-context protocols.
Thanks to the open-source community for collaborative feedback.

---

### References

* RFC 8259 — *The JSON Data Interchange Syntax*
* ECMA-404 — *The JSON Data Interchange Format*
* ISO/IEC 21778:2017 — *Information technology — JSON*
* JSON Schema 2020-12 — *Core and Validation Specifications*

---

### Authors’ Addresses

Agents.JSON Project
Email: [spec@agentsjson.org](mailto:spec@agentsjson.org)
URL: [https://agentsjson.org](https://agentsjson.org)

---

### Full Copyright Statement

Copyright © 2025 Agents.JSON Project.
This document may be copied and distributed freely with attribution.

