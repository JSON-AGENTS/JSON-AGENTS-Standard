# Agents.JSON  
## Portable Agent Manifest (PAM) Specification  
Category: Standards Track  
November 2025  

---

## Status of this Memo

This document defines the **Agents.JSON - Portable Agent Manifest (PAM)**, a JSON-based standard for describing AI agents and agent graphs in a portable, schema-driven format.

This specification is distributed for public implementation, review, and discussion.  
Distribution is unlimited.

---

## Copyright Notice

Copyright © 2025 Agents.JSON.  
All Rights Reserved.

---

## Abstract

**Agents.JSON** defines the **Portable Agent Manifest (PAM)** — a universal, JSON-native format for representing AI agents, their capabilities, tools, runtimes, and governance metadata.  

PAM provides a single, interoperable structure that allows agents to be described, validated, and exchanged across frameworks, platforms, and ecosystems without code translation.

PAM is derived entirely from established **JSON and JSON Schema** standards and designed for compatibility, governance, and multi-agent orchestration.

---

## Table of Contents

1. Introduction  
2. Requirements and Conventions  
3. Terminology  
4. Design Overview  
5. Top-Level Manifest Structure  
6. Core Profile (`core`)  
7. Exec Profile (`exec`)  
8. Gov Profile (`gov`)  
9. Graph Profile (`graph`)  
10. Extensions and Namespaces  
11. Conformance  
12. Security Considerations  
13. Example Manifests  
14. Registry Considerations  
15. IANA Considerations  
16. References  
Appendix A. Normative JSON Schema

---

## 1. Introduction

### 1.1 Motivation

The agent ecosystem has evolved through a patchwork of incompatible manifest formats — each framework defining unique metadata structures, tool schemas, and governance models.  
This fragmentation prevents portability, interoperability, and consistent validation.

Agents.JSON introduces the **Portable Agent Manifest (PAM)** as a shared schema for defining AI agents declaratively.  
The goal is to make agents:

- **Framework-agnostic**
- **Interoperable across runtimes**
- **Governable at scale**
- **Composable into networks**

### 1.2 Scope

PAM defines:

- A top-level JSON structure for agent manifests.  
- Four modular profiles:
  - `core` — identity, capabilities, tools, context.
  - `exec` — runtime and environment.
  - `gov` — security, policies, observability.
  - `graph` — multi-agent orchestration and routing.
- Validation through JSON Schema.
- A reserved extension model for innovation.

### 1.3 Non-Goals

PAM does **not** define:

- A transport or API protocol.  
- Prompt formatting or reasoning algorithms.  
- Scheduling, execution order, or model architecture.

---

## 2. Requirements and Conventions

### 2.1 Requirements Language

The key words **MUST**, **SHOULD**, and **MAY** are to be interpreted as described in RFC 2119.

### 2.2 JSON and Encoding

- Manifests MUST conform to RFC 8259, ECMA-404, and ISO/IEC 21778.  
- Encoding MUST be UTF-8.  
- Validation MUST be compatible with JSON Schema 2020-12.

---

## 3. Terminology

| Term | Definition |
|------|-------------|
| **Agent** | A logical AI entity capable of processing input, invoking tools, and generating output. |
| **Manifest** | A JSON document describing an agent or network of agents. |
| **Profile** | A modular subset of the specification (e.g., core, exec, gov, graph). |
| **Capability** | A declared function or skill the agent provides. |
| **Tool** | An external callable resource (API, function, system, or plugin). |
| **Runtime** | The environment in which the agent executes. |
| **Policy** | A declarative constraint governing behavior. |
| **Security** | Configuration related to sandboxing and access control. |
| **Graph** | A collection of interconnected agents and message routes. |
| **Message Envelope** | A structured format for messages passed between agents. |
| **Extension** | A namespaced addition to the standard manifest fields. |

---

## 4. Design Overview

### 4.1 Profiles

Manifests MAY declare one or more profiles:

```json
"profiles": ["core", "exec", "gov", "graph"]
````

If omitted, `core` is assumed.

Each profile adds semantic layers:

* **Core**: Base identity and capabilities.
* **Exec**: Runtime execution context.
* **Gov**: Governance, security, and observability.
* **Graph**: Multi-agent orchestration.

### 4.2 Manifest and Instance Relationship

A manifest describes a **template**.
Implementations MAY instantiate one or more live agents based on it.

---

## 5. Top-Level Manifest Structure

A conforming manifest MUST include at least:

```json
{
  "manifest_version": "1.0",
  "profiles": ["core"],
  "agent": {},
  "capabilities": [],
  "tools": [],
  "modalities": {},
  "context": {}
}
```

Top-level fields:

| Field              | Type   | Description                       |
| ------------------ | ------ | --------------------------------- |
| `manifest_version` | string | Identifies specification version. |
| `profiles`         | array  | Declares active profiles.         |
| `agent`            | object | Agent metadata.                   |
| `capabilities`     | array  | Agent functions.                  |
| `tools`            | array  | External callable resources.      |
| `modalities`       | object | Input/output formats.             |
| `context`          | object | Memory and windowing hints.       |

---

## 6. Core Profile (`core`)

### 6.1 `agent`

```json
"agent": {
  "id": "ajson://example/echo",
  "name": "Echo Agent",
  "description": "Echoes back text.",
  "version": "1.0.0",
  "license": "Apache-2.0"
}
```

### 6.2 `capabilities`

```json
"capabilities": [
  { "id": "echo", "description": "Echo input text." }
]
```

### 6.3 `tools`

```json
"tools": [
  {
    "id": "tool://search",
    "name": "Search API",
    "type": "http",
    "endpoint": "https://api.example.com/search",
    "input_schema": { "type": "object" },
    "output_schema": { "type": "object" }
  }
]
```

### 6.4 `modalities`

```json
"modalities": {
  "input": ["text"],
  "output": ["text", "json"]
}
```

### 6.5 `context`

```json
"context": { "window": 4096, "persistent": false }
```

---

## 7. Exec Profile (`exec`)

### 7.1 `runtime`

```json
"runtime": {
  "type": "python",
  "entrypoint": "main:run",
  "env": { "MODE": "production" },
  "resources": { "cpu_cores_min": 1, "memory_mb_min": 512 }
}
```

---

## 8. Gov Profile (`gov`)

### 8.1 `security`

```json
"security": {
  "sandbox": "container",
  "network_zone": "trusted"
}
```

### 8.2 `policies`

```json
"policies": [
  {
    "id": "deny-external-http",
    "effect": "deny",
    "action": "tool.call",
    "where": "tool.endpoint !~ 'internal'"
  }
]
```

### 8.3 `observability`

```json
"observability": {
  "log_level": "info",
  "metrics_enabled": true
}
```

---

## 9. Graph Profile (`graph`)

### 9.1 `graph.nodes`

```json
"graph": {
  "nodes": [
    { "id": "router", "ref": "ajson://example/router" },
    { "id": "faq", "ref": "ajson://example/faq" }
  ]
}
```

### 9.2 `graph.edges`

```json
"graph": {
  "edges": [
    { "from": "router", "to": "faq", "condition": "message.intent == 'faq'" }
  ]
}
```

### 9.3 Message Envelope

```json
"graph": {
  "message_envelope": {
    "schema": "https://agents.json.org/schema/message-envelope.json"
  }
}
```

---

## 10. Extensions and Namespaces

### 10.1 `extensions`

```json
"extensions": {
  "vendor.example/routing": { "shadow_mode": true }
}
```

### 10.2 `x-*`

```json
"x-internal": { "notes": "for testing only" }
```

Implementations MUST ignore unrecognized extensions.

---

## 11. Conformance

A manifest is conformant if:

* It validates against the Agents.JSON schema.
* Declared profiles match included sections.
* No unknown non-extension fields are present.

---

## 12. Security Considerations

* Manifests MUST NOT embed secrets.
* Evaluators MUST NOT execute manifest content.
* Policy failures SHOULD fail closed.

---

## 13. Example Manifests

### 13.1 Minimal

```json
{
  "manifest_version": "1.0",
  "agent": { "id": "ajson://example/minimal", "name": "Minimal" }
}
```

### 13.2 Full Example

```json
{
  "manifest_version": "1.0",
  "profiles": ["core", "exec", "gov", "graph"],
  "agent": { "id": "ajson://example/router", "name": "Router" },
  "runtime": { "type": "node", "entrypoint": "dist/router.js" },
  "security": { "sandbox": "process" },
  "graph": {
    "nodes": [
      { "id": "router", "ref": "ajson://example/router" },
      { "id": "faq", "ref": "ajson://example/faq" }
    ],
    "edges": [
      { "from": "router", "to": "faq", "condition": "message.intent == 'faq'" }
    ]
  }
}
```

---

## 14. Registry Considerations

Implementations MAY maintain public registries for:

* Capabilities
* Tool types
* Profiles
* Envelope schemas

---

## 15. IANA Considerations

### 15.1 Media Type

Agents.JSON manifests SHOULD use the media type:

```
application/agents+json
```

This media type is intended for registration with IANA and follows the structured syntax suffix convention defined in RFC 6838.

### 15.2 File Extension

The recommended file extension for Agents.JSON manifests is:

```
.agents.json
```

This extension clearly identifies agent manifest files while maintaining compatibility with standard JSON tooling.

### 15.3 Content Negotiation

Servers and clients MAY use standard HTTP content negotiation to request or serve Agents.JSON manifests:

```http
Accept: application/agents+json
Content-Type: application/agents+json
```

---

## 16. References

* RFC 8259 — The JavaScript Object Notation (JSON) Data Interchange Format
* ECMA-404 — The JSON Data Interchange Syntax
* ISO/IEC 21778:2017 — JSON Data Interchange Syntax
* JSON Schema 2020-12
* JSON-LD 1.1
* RFC 6838 — Media Type Specifications and Registration Procedures

---

## Appendix A. Normative JSON Schema

See [`schema/agents-json.json`](./schema/agents-json.json) for the normative schema definition.

