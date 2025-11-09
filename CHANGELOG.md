# Changelog  
All notable changes to the Agents.JSON specification will be documented in this file.

This project adheres to [Semantic Versioning 2.0.0](https://semver.org/).

---

## [Unreleased]

### Added
- Placeholder section for upcoming proposals and drafts.

### Changed
- No pending changes at this time.

### Fixed
- None yet.

---

## [1.0.0] — 2025-11-09  
**Initial Publication**

### Added
- **Agents.JSON Specification (`agents-json.md`)**  
  - Defines the **Portable Agent Manifest (PAM)** model.  
  - Includes normative structure for `core`, `exec`, `gov`, and `graph` profiles.  
  - Provides formal terminology, examples, and conformance sections.

- **Canonical JSON Schema (`schema/agents-json.json`)**  
  - Implements validation for all core fields and conditional profile requirements.  
  - Supports `extensions` and `x-*` namespaces.  
  - Ensures JSON Schema 2020-12 compatibility.

- **Repository Layout and Supporting Files**  
  - `README.md` with overview and quick reference.  
  - `LICENSE` under Apache 2.0.  
  - `CONTRIBUTING.md` for guidelines and validation workflow.  
  - `CODE_OF_CONDUCT.md` with Contributor Covenant 2.0.
  - Directory structure for `/schema`, `/examples`, `/registry`, `/docs`.

- **Extension System**
  - Extension registry (`registry/extensions.json`) with 4 registered extensions.
  - Schema definitions for `x-audit` and `x-memory` extensions.
  - Comprehensive extensions guide (`docs/extensions.md`).

- **Profile Registry**
  - Formal profile definitions (`registry/profiles.json`).
  - Documentation for extending with custom profiles.

- **Capability and Tool Registries**
  - Capabilities registry with 7 standard capabilities.
  - Tool types registry with 6 recognized types.
  - Schema definitions for summarization, routing, and retrieval capabilities.

- **Interoperability Mappings**
  - Framework mapping guide (`docs/mapping-frameworks.md`).
  - Conversion patterns for LangChain, OpenAI, AutoGen, MCP, and others.
  - YAML ↔ JSON interoperability guidance.

- **IANA Considerations**
  - Media type: `application/agents+json`.
  - File extension: `.agents.json`.
  - Content negotiation guidelines.

### Notes
This release defines the foundation for all subsequent versions.  
Backward compatibility will be preserved whenever feasible.

---

## Versioning Policy

- **Patch** — editorial, clarification, or non-breaking corrections.  
- **Minor** — new optional fields or example additions.  
- **Major** — breaking schema or terminology changes.

---

## Future Considerations

| Area | Potential for v1.1+ |
|------|---------------------|
| **Additional Capabilities** | Schemas for qa, classification, extraction, generation capabilities |
| **Extension Schemas** | Formal schemas for x-embeddings and x-research extensions |
| **Profile Extensions** | Community-defined profiles (e.g., realtime, evaluation, data-plane) |
| **Validation Tooling** | Reference implementation validators and converters |
| **URI Scheme** | Formal definition and resolution mechanism for `ajson://` URIs |
| **Policy Expression Language** | Standardized expression syntax for policy `where` clauses |

---

**© 2025 Agents.JSON. All rights reserved.**
