# .ai — Workflow Prompt Templates

This folder contains **role-specific prompt templates** used by the AI Delivery Engine workflow.

These templates define how AI roles perform planning, building, reviewing, backlog generation, and repository discovery tasks.

---

## Templates

| File | Role | Purpose |
|---|---|---|
| `planner-template.md` | Planner | Converts an issue or request into a structured feature documentation set |
| `builder-template.md` | Builder | Implements a feature strictly from the documentation set |
| `reviewer-template.md` | Reviewer | Validates the PR against documentation and acceptance criteria |
| `backlog-generator-template.md` | Backlog Generator | Converts a brief or audit into prioritised GitHub issues |
| `current-state-template.md` | Current State Analyst | Produces a verified snapshot of repository structure and capabilities |

---

## How this relates to the main README

This file is a quick operating guide for the prompt templates.

For system overview, orchestration patterns, lifecycle diagrams, and governance, see the **root `README.md`**.

---

## How to use

### 1) Manual prompt usage

Copy a template into your AI assistant context and provide the required inputs.

Example:

```text
@workspace
@file .ai/planner-template.md
@file docs/current-state.md

Task: Plan GitHub issue #42
```

### 2) CLI-based invocation

Templates can be piped into AI CLI tools.

```bash
cat .ai/planner-template.md | claude
```

### 3) Agent orchestration

Templates can be invoked via optional agent files located in:

```text
agents
```

---

## Artefact authority model

Templates assume the following artefact authority hierarchy:

1. `decisions.md` (authoritative)
2. `contract.md`
3. `spec.md` / `acceptance.md`
4. `checklist.md`

Builders and Reviewers must resolve conflicts using this hierarchy.

---

## Customisation

These templates are intentionally generic. Adapt them to your project standards:

- Naming conventions
- Repo patterns and boundaries
- Security requirements
- Testing expectations
- Integration safeguards

---

## Versioning and governance

Treat templates as workflow infrastructure:

- Change via pull requests
- Avoid silent behavioural changes
- Prefer small, well-described edits