## Feature identity

- **GitHub Issue:** #<!-- number -->
- **Feature name:** <!-- kebab-case feature-name -->
- **Feature folder:** `docs/features/<feature-name>/`
- **PR stage label:** `ready-for-review` / `ready-for-merge`

## Summary

<!-- Brief description of what this PR does and why. -->

## Artefacts and acceptance mapping

**Planner artefacts (required):**
- `docs/features/<feature-name>/spec.md`
- `docs/features/<feature-name>/contract.md`
- `docs/features/<feature-name>/acceptance.md`
- `docs/features/<feature-name>/checklist.md`
- Optional: `decisions.md`, `open-questions.md`

**Artefact authority:** `decisions.md` → `contract.md` → `spec.md`/`acceptance.md` → `checklist.md`

| Acceptance criterion (from acceptance.md) | Status | Evidence (tests/commands/screenshots/notes) |
|---|---|---|
| <!-- Given/When/Then item --> | :white_check_mark: / :x: | <!-- how it was verified --> |

## Risk notes

- **Breaking changes:** None / _describe_
- **Performance impact:** None / _describe_
- **Security considerations:** None / _describe_

## Migration notes

- **DB migrations:** None / list migration files or commands (e.g., `path/to/migrations/*.sql`)
- **Config/env changes:** None / list keys + where set
- **Backwards compatibility:** None / describe
- **Rollback plan:** None / describe

## Checklist

- [ ] Feature identity completed (issue, feature name, feature folder)
- [ ] Planner artefacts referenced above and aligned (authority respected)
- [ ] Acceptance criteria mapped above with evidence
- [ ] Tests cover new/changed behaviour
- [ ] Verification commands run (paste output links or short notes):
  - [ ] Local dev runs: `npm run dev` (as applicable)
  - [ ] Static site builds successfully: `bundle exec jekyll build` (locally or via CI)
  - [ ] Any additional tests / checks (e.g., integration tests / DB tests), as applicable
- [ ] No secrets or credentials committed
- [ ] Documentation updated (README/current-state/feature docs) if applicable
- [ ] Post-merge actions noted (deploy/migration apply/monitoring)

## Reviewer sign-off

- **Reviewer outcome:** APPROVE / REQUEST CHANGES / ESCALATE
- **Issue comment posted:** Yes / No (link)
- **Label transition:** `ready-for-review` → `ready-for-merge` (if approved)
