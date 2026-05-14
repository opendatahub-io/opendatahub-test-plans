# opendatahub-test-plans

Central repository for test plans and test case specifications for [Red Hat OpenShift AI (RHOAI)](https://www.redhat.com/en/technologies/cloud-computing/openshift/openshift-ai) and [Open Data Hub (ODH)](https://opendatahub.io/).

Test plans document the test strategy, approach, and individual test cases for each feature. Automated tests based on these plans are implemented upstream in component repos (e.g., [kubeflow/hub](https://github.com/kubeflow/hub)) and downstream in [opendatahub-tests](https://github.com/opendatahub-io/opendatahub-tests).

## Repository Structure

```
opendatahub-test-plans/
├── README.md
├── CONTRIBUTING.md
├── plans/                          # All test plans live here
│   ├── <team_name>/                # One folder per team
│   │   ├── <feature_name>/         # One folder per feature
│   │   │   ├── README.md           # Feature overview, links to ADR/Epic/STRAT
│   │   │   ├── TestPlan.md         # Full test strategy and execution plan
│   │   │   ├── TestPlanGaps.md     # Open questions / gaps (optional)
│   │   │   ├── TestPlanReview.md   # Review results (optional)
│   │   │   └── test_cases/
│   │   │       ├── INDEX.md        # Test case index by category
│   │   │       └── TC-<CAT>-NNN.md # Individual test case specs
│   │   └── <another_feature>/
│   │       └── ...
│   └── <another_team>/
│       └── ...
```

## Getting Started

See the [Contributing Guide](CONTRIBUTING.md) for how to add or update test plans.
