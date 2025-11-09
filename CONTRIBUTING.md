# Contributing to Agents.JSON

Thank you for your interest in improving **Agents.JSON** — the open specification for defining portable AI agents.

This document explains how to propose changes, report issues, and contribute new content to the repository.

---

## 📜 Guiding Principles

1. **Neutrality:**  
   Agents.JSON aims to remain framework-agnostic and vendor-neutral.

2. **Clarity over complexity:**  
   Favor explicit field definitions, clear JSON examples, and human-readable schema descriptions.

3. **Backward compatibility:**  
   All revisions should strive to remain compatible with previously valid manifests whenever possible.

4. **Extensibility:**  
   Use the `extensions` or `x-*` namespaces for experimentation and forward-looking ideas.

---

## 🧭 Ways to Contribute

- **Improve documentation:** fix typos, clarify examples, or improve formatting.  
- **Propose schema changes:** add or refine properties in the JSON Schema.  
- **Suggest new profiles:** expand functionality while keeping the core minimal.  
- **Add examples:** new agent configurations, tool schemas, or graph topologies.  
- **Discuss interoperability:** map how existing frameworks could align with Agents.JSON.

---

## ⚙️ Development Setup

1. Fork the repository.
2. Create a new branch for your contribution:
   ```bash
   git checkout -b feature/your-change
````

3. Make your edits and run a JSON Schema validation test:

   ```bash
   npm install -g ajv-cli
   ajv validate -s schema/agents-json.json -d examples/*.json
   ```
4. Commit and push your changes:

   ```bash
   git commit -m "Update schema: added 'metrics_enabled' property"
   git push origin feature/your-change
   ```
5. Open a pull request describing your proposal.

---

## 🧪 Validation

All pull requests must include:

* A **valid JSON Schema** that passes `ajv` validation.
* Updated or new **examples** demonstrating your change.
* A note in `CHANGELOG.md` summarizing the update.

---

## 🧩 File Naming Conventions

| Type          | Directory                         | Convention                                |
| ------------- | --------------------------------- | ----------------------------------------- |
| Specification | `/agents-json.md`                 | Lowercase with hyphen                     |
| Schema        | `/schema/agents-json.json`        | One canonical schema per version          |
| Examples      | `/examples/`                      | Descriptive names (`core-exec-gov.json`)  |
| Docs          | `/docs/`                          | Markdown files for guides and mappings    |
| Registries    | `/registry/`                      | JSON lists defining canonical identifiers |

---

## 🧱 Pull Request Template

Each pull request should contain:

```markdown
### Summary
Brief explanation of the proposed change.

### Motivation
Why this change improves the standard.

### Impact
Does it affect backward compatibility?

### Example
JSON snippet demonstrating the change.
```

---

## 🧠 Discussion and Governance

Decisions are reached through open discussion and consensus within GitHub issues and pull requests.
Major changes (e.g., new profiles or schema rewrites) should begin as **Proposals** in `/docs/proposals/`.

---

## ⚖️ Licensing

All contributions are licensed under the **Apache 2.0 License**, as stated in the root `LICENSE` file.
By contributing, you agree that your submissions will be released under this license.

---

## 🧾 Code of Conduct

All contributors are expected to follow professional and respectful communication practices.
See [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md) for more information.

---

Thank you for helping shape the future of portable AI agent interoperability.

