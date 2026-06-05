# Welcome to opendatahub-test-plans contributing guide

Thank you for contributing to our project!

## New contributor guide

To get an overview of the project, read the [README](README.md).

## Adding a new test plan

You can create test plans **manually** or use the
[odh-test-gen](<https://github.com/opendatahub-io/odh-test-gen>)
skills to generate them from a Jira strategy (STRAT)
or RHOAI ticket.

### Manual creation

1. Create or use your team's directory under `plans/` (e.g., `plans/ai-hub/`, `plans/platform/`)
2. Under your team folder, create a directory named after the feature using snake_case (e.g.,
   `plans/ai-hub/mcp_catalog/`)
3. Add the following files:

| File | Required | Description |
|------|----------|-------------|
| `README.md` | Yes | Feature overview with links to ADR, Epic, and STRAT |
| `TestPlan.md` | Yes | Full test strategy and execution plan |
| `TestPlanGaps.md` | No | Open questions or gaps identified during planning |
| `TestPlanReview.md` | No | Review results and scores |
| `test_cases/INDEX.md` | Yes | Index of all test cases organized by category |
| `test_cases/TC-<CAT>-NNN.md` | Yes | Individual test case specifications |

### Using odh-test-gen

From [odh-test-gen](https://github.com/opendatahub-io/odh-test-gen), use the following skills:

- `/test-plan-create` — Generate a test plan from a Jira strategy
- `/test-plan-create-cases` — Generate individual test case files from an existing test plan
- `/test-plan-publish` — Publish test plan artifacts to this repository via PR

### Test case naming convention

Test cases follow the `TC-<CATEGORY>-<NUMBER>.md` pattern:

- `TC-API-001.md` — API test cases
- `TC-SEC-001.md` — Security test cases
- `TC-E2E-001.md` — End-to-end test cases
- `TC-ERROR-001.md` — Error handling test cases
- `TC-DATA-001.md` — Data validation test cases
- `TC-LOAD-001.md` — YAML/data loading test cases
- `TC-REG-001.md` — Regression test cases

Use categories that make sense for your feature. Keep numbering sequential within each category.

## Issues

If you find a problem, [search if an issue already exists](https://github.com/opendatahub-io/opendatahub-test-plans/issues).
If a related issue doesn't exist, you can open a new
[issue](https://github.com/opendatahub-io/opendatahub-test-plans/issues/new).

## Pull requests

1. Fork the repository
2. Create a branch from `main`
3. Add or update your test plan following the structure above
4. Open a pull request for review
5. Request review from your team lead or peers
