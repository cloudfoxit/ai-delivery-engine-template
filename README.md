# ai-delivery-engine-template

A portable template for running a **Planner → Builder → Reviewer** workflow with AI coding assistants. It uses feature-folder documentation as the source of truth, with optional agent orchestration patterns.

## Why use this?

AI coding assistants are powerful but undirected. Without structure, they produce code that drifts from requirements, skips edge cases, and is hard to review. This template solves that by introducing a lightweight delivery workflow:

1. **Planner** — reads a GitHub issue and produces a documentation set (spec, contracts, acceptance criteria, checklist, decisions, open questions).
2. **Builder** — implements the feature by following the documentation set exactly.
3. **Reviewer** — verifies the pull request against the documentation set.

The documentation set acts as a shared contract between all three roles. This keeps AI assistants on track and gives human reviewers a clear reference point.

## Quickstart

### 1. Copy into your project

```bash
# From your project root
cp -r path/to/ai-delivery-engine-template/.ai .ai
```

That's it. The `.ai/` folder contains the prompt templates. Everything else in this repo (agents, examples, GitHub templates) is optional.

### 2. Create a GitHub issue

Write a feature request or chore with clear acceptance criteria. Label it `ready-for-planning`.

```bash
gh issue create \
  --title "Add password reset flow" \
  --label "feature,ready-for-planning" \
  --body "## Summary
As a user, I want to reset my password so that I can regain access to my account.

## Acceptance criteria
- [ ] User can request a reset email
- [ ] Reset link expires after 1 hour
- [ ] User can set a new password via the link"
```

### 3. Run the Planner

Feed the planner template and issue to your AI assistant:

```bash
cat .ai/planner-template.md  # copy into your AI assistant's context
```

The Planner produces a feature folder:

```text
docs/features/password-reset/
├─ spec.md
├─ contract.md
├─ acceptance.md
├─ checklist.md
├─ decisions.md
└─ open-questions.md
```

### 4. Gate check

Review `open-questions.md`. Any question marked `status: blocking` must be resolved before proceeding. This is a hard gate — the Builder should not start work while blocking questions remain.

### 5. Run the Builder

Feed the builder template and the documentation set to your AI assistant. The Builder works through `checklist.md` in order, then opens a pull request.

### 6. Run the Reviewer

Feed the reviewer template, the PR diff, and the documentation set to your AI assistant. The Reviewer checks the implementation against every acceptance criterion and either approves or requests changes.

## Folder structure

```text
your-project/
├─ .ai/                          # Workflow prompt templates (copy this)
│  ├─ planner-template.md
│  ├─ builder-template.md
│  ├─ reviewer-template.md
│  ├─ backlog-generator-template.md
│  ├─ current-state-template.md
│  └─ README.md
├─ docs/features/                # Feature documentation (created per feature)
│  └─ <feature-name>/
│     ├─ spec.md                 # What the feature does
│     ├─ contract.md             # API/interface contracts
│     ├─ acceptance.md           # Testable acceptance criteria
│     ├─ checklist.md            # Ordered implementation tasks
│     ├─ decisions.md            # Architecture decision records
│     └─ open-questions.md       # Unresolved questions
└─ ...
```

## Feature folder documentation set

Each feature gets its own folder under `docs/features/<feature-name>/` containing six files:

| File | Purpose |
| --- | --- |
| `spec.md` | Functional specification — what the feature does, user stories, edge cases |
| `contract.md` | API and interface contracts — endpoints, schemas, events, type definitions |
| `acceptance.md` | Acceptance criteria in Given/When/Then format, each independently testable |
| `checklist.md` | Ordered implementation tasks for the Builder to follow |
| `decisions.md` | Architecture decision records — options considered, chosen approach, rationale |
| `open-questions.md` | Unresolved questions with `status: blocking` or `status: non-blocking` markers |

See [`examples/sample-feature/`](examples/sample-feature/) for a complete worked example.

## Artefact authority hierarchy

The feature documentation set is the **source of truth**. When there is a conflict:

```text
spec.md + contract.md  >  GitHub issue comments  >  verbal/chat discussion
```

If a spec contradicts an issue comment, the spec wins (once approved). If something in the issue is not reflected in the spec, the Planner should update the spec — not the other way around.

This hierarchy exists because the documentation set is reviewed, versioned, and structured. Issue comments and chat messages are not.

## Open questions and blocking gates

Open questions are tracked in `open-questions.md` within each feature folder. Each question has a status:

- **`status: blocking`** — this question must be resolved before the Builder can work on tasks that depend on it. This is a hard gate.
- **`status: non-blocking`** — this question does not prevent progress. The Builder may proceed with a reasonable default and note the assumption.
- **`status: resolved`** — include the resolution so it serves as a decision record.

The gate check between Planner and Builder is the most important quality control point. If blocking questions slip through, the Builder will either guess (producing wrong code) or stall.

## Issue label flow

Issues move through a five-stage label flow:

```text
ready-for-planning → ready-for-build → ready-for-review → ready-for-merge → done
```

| Label | Meaning |
| --- | --- |
| `ready-for-planning` | Issue accepted, waiting for Planner to produce documentation |
| `ready-for-build` | Documentation set complete, Builder may start |
| `ready-for-review` | PR opened, Reviewer may start |
| `ready-for-merge` | Review approved, PR may be merged |
| `done` | Merged and issue closed |

Label transitions are performed by the role completing their phase:

- **Planner:** `ready-for-planning` → `ready-for-build`
- **Builder:** `ready-for-build` → `ready-for-review`
- **Reviewer:** `ready-for-review` → `ready-for-merge`
- **Merge (manual or CI):** `ready-for-merge` → `done`

### CLI commands for label transitions

```bash
# Planner done
gh issue edit <number> --remove-label "ready-for-planning" --add-label "ready-for-build"

# Builder done
gh issue edit <number> --remove-label "ready-for-build" --add-label "ready-for-review"

# Reviewer approves
gh issue edit <number> --remove-label "ready-for-review" --add-label "ready-for-merge"

# After merge
gh issue edit <number> --remove-label "ready-for-merge" --add-label "done"
gh issue close <number>
```

## CLI-first guidance

This workflow is designed to be run from the command line. Use `gh` (GitHub CLI) for issue and PR management, and feed prompt templates to your AI assistant via copy-paste or piping.

**MCP (Model Context Protocol) is an optional fallback** for GitHub admin tasks (creating issues, managing labels, reviewing PRs). If your AI assistant supports MCP and you find it more convenient, you may use it — but the workflow does not require it.

Prefer CLI because:

- It works with any AI assistant, not just those with MCP support.
- Commands are explicit, auditable, and scriptable.
- There is no dependency on a running MCP server.

## Agent orchestration (optional)

The `agents/` folder contains `.agent.md` files for AI tools that support agent definitions:

| File | Role |
| --- | --- |
| `conductor.agent.md` | Optional orchestrator that coordinates the full workflow |
| `planning-subagent.agent.md` | Planner role as a sub-agent |
| `implement-subagent.agent.md` | Builder role as a sub-agent |
| `code-review-subagent.agent.md` | Reviewer role as a sub-agent |

These are entirely optional. The core workflow works without them — they are useful if your AI tool supports multi-agent orchestration.

### Placement

Some tools expect agent files in the repository root. If so, copy them:

```bash
cp agents/*.agent.md .
```

Other tools may support a nested `agents/` folder natively. Use whichever convention your tool expects.

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

## Licence

[MIT](LICENSE) — Copyright (c) 2026 Cloudfox Group Limited
