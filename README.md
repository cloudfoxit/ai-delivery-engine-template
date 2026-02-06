## Artefact authority hierarchy

The feature documentation set is the **source of truth**. When there is a conflict between artefacts, the following hierarchy applies:

```text
decisions.md  >  contract.md  >  spec.md + acceptance.md  >  checklist.md  >  GitHub issue comments  >  verbal/chat discussion
```

- **decisions.md is authoritative** for architectural and behavioural decisions.
- If downstream artefacts contradict decisions.md, they MUST be updated to match it.
- If a requirement exists in an issue but is not reflected in the documentation set, the Planner MUST update the documentation — never the implementation.

This hierarchy exists because the documentation set is reviewed, versioned, and structured. Issue comments and chat discussions are not.

---

## Derived Capability Mapping

Each feature folder MAY include a **Derived Capability** section inside `spec.md`. This maps low‑level technical implementation to user‑facing or business‑facing capabilities.

Purpose:

- Helps backlog generation identify missing capabilities
- Improves traceability between business goals and implementation
- Enables orchestration agents to reason about system behaviour instead of only code

Typical mapping examples:

```text
Database tables → Customer onboarding capability
Webhook handler → Payment lifecycle capability
UI flow → Self‑service customer management capability
```

Derived Capability is OPTIONAL but strongly recommended for:

- Marketplace platforms
- SaaS onboarding flows
- Billing and subscription systems
- Multi‑service orchestration workflows

---

## Current State and Backlog Generation Workflow

This template supports discovery‑driven planning using two additional artefacts:

### current-state.md

Generated using `current-state-template.md`, this file provides a verified snapshot of:

- Architecture and module responsibilities
- Data models and API surfaces
- External integrations and dependencies
- Capability mapping (system behaviours exposed by current implementation)

This ensures Planners and Backlog Generators operate using factual system context rather than assumptions.

### Backlog Generation

The `backlog-generator-template.md` uses:

- `current-state.md`
- Product briefs or feature goals

To generate:

- Capability gap analysis
- Prioritised implementation issues
- Dependency‑aware feature sequencing

This enables a **Discovery → Planning → Implementation → Review** delivery lifecycle.

---

## Agent orchestration (optional)

The `agents/` folder contains `.agent.md` files for AI tools that support agent definitions and orchestration workflows.

| File | Role |
| --- | --- |
| `conductor.agent.md` | Optional orchestrator coordinating Planner → Builder → Reviewer lifecycle and enforcing label‑based delivery gates |
| `planning-subagent.agent.md` | Planner role as a sub‑agent |
| `implement-subagent.agent.md` | Builder role as a sub‑agent |
| `code-review-subagent.agent.md` | Reviewer role as a sub‑agent |

### Advanced orchestration extensions

Projects may optionally introduce additional specialized agents to improve context efficiency and parallel execution:

| Agent | Purpose |
| --- | --- |
| `explorer-subagent.agent.md` | Fast codebase discovery, file mapping, dependency scanning |
| `oracle-subagent.agent.md` | Deep architecture analysis and pattern recognition |

These agents help reduce context overload by isolating discovery and analysis from implementation.

### Human‑in‑the‑Loop Governance

Even when orchestration agents are used, this workflow intentionally preserves human approval gates:

- Planner output must be reviewed before Builder begins
- Blocking open questions must be resolved before implementation
- Reviewer approval is required before merge
- Issue closure occurs only after merge and deployment validation

Orchestration agents MAY assist with delivery, but final governance remains human‑controlled.

### YAML Frontmatter Convention

Agent files may include YAML metadata at the top of each `.agent.md` file:

```
---
description: Purpose of the agent
model: Preferred AI model
tools: [search, edit, runCommands]
handoff:
  - agent: <target-agent>
    description: When this handoff should occur
---
```

This enables:

- Tool governance
- Model routing
- Structured delegation
- Future compatibility with advanced orchestration engines

## Delivery Governance vs Coding Orchestration

This template is intentionally governance‑first. It focuses on ensuring that features are:

- Fully specified before implementation
- Traceable from requirements to code
- Auditable through structured artefacts

Agent orchestration frameworks (such as Orchestra and Atlas) optimise *how* AI writes code. This template optimises *how software is safely delivered*. The two approaches are complementary and can be combined.

## CLI-first guidance

This workflow is designed to be run from the command line. Use `gh` (GitHub CLI) for issue and PR management, and feed prompt templates to your AI assistant via copy-paste or piping.

**MCP (Model Context Protocol) is an optional fallback** for GitHub admin tasks (creating issues, managing labels, reviewing PRs). If your AI assistant supports MCP and you find it more convenient, you may use it — but the workflow does not require it.

Prefer CLI because:

- It works with any AI assistant, not just those with MCP support.
- Commands are explicit, auditable, and scriptable.
- There is no dependency on a running MCP server.
- Supports deterministic automation and CI/CD integration

### Parallel Execution Guidance

Builders and orchestration agents MAY execute independent checklist tasks in parallel when:

- Tasks have no shared schema or contract dependencies
- Acceptance criteria remain independently testable
- Migration ordering remains deterministic

Parallel execution is optional and should favour correctness over speed.

## Agent orchestration (optional)

The `agents/` folder contains `.agent.md` files for AI tools that support agent definitions and orchestration workflows.

| File | Role |
| --- | --- |
| `conductor.agent.md` | Optional orchestrator coordinating Planner → Builder → Reviewer lifecycle and enforcing label‑based delivery gates |
| `planning-subagent.agent.md` | Planner role as a sub‑agent |
| `implement-subagent.agent.md` | Builder role as a sub‑agent |
| `code-review-subagent.agent.md` | Reviewer role as a sub‑agent |

### Advanced orchestration extensions

Projects may optionally introduce additional specialized agents to improve context efficiency and parallel execution:

| Agent | Purpose |
| --- | --- |
| `explorer-subagent.agent.md` | Fast codebase discovery, file mapping, dependency scanning |
| `oracle-subagent.agent.md` | Deep architecture analysis and pattern recognition |

These agents help reduce context overload by isolating discovery and analysis from implementation.

### Human‑in‑the‑Loop Governance

Even when orchestration agents are used, this workflow intentionally preserves human approval gates:

- Planner output must be reviewed before Builder begins
- Blocking open questions must be resolved before implementation
- Reviewer approval is required before merge
- Issue closure occurs only after merge and deployment validation

Orchestration agents MAY assist with delivery, but final governance remains human‑controlled.

### YAML Frontmatter Convention

Agent files may include YAML metadata at the top of each `.agent.md` file:

```
---
description: Purpose of the agent
model: Preferred AI model
tools: [search, edit, runCommands]
handoff:
  - agent: <target-agent>
    description: When this handoff should occur
---
```

This enables:

- Tool governance
- Model routing
- Structured delegation
- Future compatibility with advanced orchestration engines

## Delivery Governance vs Coding Orchestration

This template is intentionally governance‑first. It focuses on ensuring that features are:

- Fully specified before implementation
- Traceable from requirements to code
- Auditable through structured artefacts

Agent orchestration frameworks (such as Orchestra and Atlas) optimise *how* AI writes code. This template optimises *how software is safely delivered*. The two approaches are complementary and can be combined.

## What is included in this template

| Path | Purpose |
| --- | --- |
| `.ai/` | Prompt templates for each workflow role |
| `agents/` | Optional agent definition files |
| `examples/sample-feature/` | Worked example of a feature documentation set |
| `.github/pull_request_template.md` | PR template with acceptance mapping and risk notes |
| `.github/ISSUE_TEMPLATE/` | Issue templates for feature requests and chores |
| `.editorconfig` | Consistent formatting across editors |
| `.gitignore` | Ignores generated artefacts (`/plans`) |

## What is NOT included

- **No application code.** This is a workflow template, not a starter kit.
- **No CI/CD configuration.** Add your own based on your tech stack.
- **No generated artefacts.** The `/plans` directory is gitignored. Do not commit planning outputs.
- **No tech-stack opinions.** The templates work with any language, framework, or platform.

## Customisation

To adapt this template to your project:

1. **Add project-specific rules** to the prompt templates (e.g., "use repository pattern", "all endpoints require authentication").
2. **Adjust the documentation set** if your project needs different artefacts (e.g., add `migration.md` for database-heavy features).
3. **Add or remove review dimensions** in `reviewer-template.md` to match your quality bar.
4. **Create a `current-state.md`** using the current-state template to give Planners context about your codebase.
5. **Define Derived Capability conventions** if your product requires structured capability tracking for backlog generation or reporting.

## Licence

[MIT](LICENSE) — Copyright (c) 2026 Cloudfox Group Limited
