# AGENTS.md

## What this repo is

Markdown-only test plans for Red Hat OpenShift AI / Open Data Hub.
No code, no builds, no tests to run.
All files under `plans/<team>/<feature>/`.

## Commit requirements

- **Conventional commits** required: types are
  `ci`, `docs`, `feat`, `fix`, `revert`, `style`. Subject 10–80 chars.
- **Signed-off-by trailer required** — always use `git commit -s`.
- Pre-commit hooks run `markdownlint --fix` automatically on staged `.md` files.

## Markdown lint rules

Config: `.markdownlint.yaml`

- Line length limit: **100 chars** (tables, code blocks, and headings are exempt).
- MD036 is disabled.

## File structure conventions

Each feature lives at `plans/<team_name>/<feature_name>/` (snake_case). Required files:

| File | Required |
|------|----------|
| `README.md` | Yes |
| `TestPlan.md` | Yes |
| `test_cases/INDEX.md` | Yes |
| `test_cases/TC-<CAT>-NNN.md` | Yes |
| `TestPlanGaps.md` | No |
| `TestPlanReview.md` | No |

Test case naming: `TC-<CATEGORY>-<NUMBER>.md`
(e.g., `TC-API-001.md`, `TC-SEC-001.md`).
Numbers are sequential within each category.

## Generation tooling

Test plans can be generated via
[odh-test-gen](https://github.com/opendatahub-io/odh-test-gen) skills:
`/test-plan-create`, `/test-plan-create-cases`, `/test-plan-publish`.
