# .ai — Workflow prompt templates

This folder contains prompt templates for each role in the AI Delivery Engine workflow.

## Templates

| File | Role | Purpose |
|---|---|---|
| `planner-template.md` | Planner | Takes an issue and produces the feature documentation set |
| `builder-template.md` | Builder | Implements the feature following the documentation set |
| `reviewer-template.md` | Reviewer | Reviews the PR against the documentation set |
| `backlog-generator-template.md` | Backlog Generator | Analyses a brief and produces prioritised GitHub issues |
| `current-state-template.md` | Current State Analyst | Produces a codebase snapshot for context |

## How to use

These templates are designed to be fed to an AI coding assistant (e.g., Claude) as system or user prompts. You can:

1. **Copy-paste** the template into your AI assistant's context.
2. **Reference via CLI** — e.g., `cat .ai/planner-template.md | claude` (piping into a CLI tool).
3. **Use with agent orchestration** — see the `agents/` folder for optional `.agent.md` files that reference these templates.

## Customisation

These templates are starting points. Adapt them to your project's conventions:

- Add project-specific rules (e.g., "always use repository pattern for data access").
- Adjust the documentation set if your project needs different artefacts.
- Add or remove review dimensions in the reviewer template.
