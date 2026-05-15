# MCP Catalog Test Plan

This folder contains the comprehensive test plan and test cases for the **MCP Catalog Backend** feature.

## 📚 Documentation

### Main Documents

- **[TestPlan.md](TestPlan.md)** - Complete test strategy, approach, and execution plan
- **[test_cases/INDEX.md](test_cases/INDEX.md)** - Index of all 69 test cases organized by category
- **[coverage-assessment.md](coverage-assessment.md)** - Assessment of test automation coverage
  and epic validation completeness

### Test Cases

All test cases are extracted into individual markdown files in the `test_cases/` directory for
easy reference and maintenance.

## 🔗 Related Links

- **ADR**: [MCP Catalog Backend ADR](https://docs.google.com/document/d/17PBwS5DzDI79eGXUkHowmlQlVSKOwXbBl__DtNRAwfU/edit?usp=sharing)
- **Epic**: [RHOAIENG-44926](https://issues.redhat.com/browse/RHOAIENG-44926)
- **Feature Story**: [RHAISTRAT-1084](https://issues.redhat.com/browse/RHAISTRAT-1084)

## 📁 Directory Structure

```text
mcp_catalog/
├── README.md                 # This file
├── TestPlan.md              # Main test plan document
├── coverage-assessment.md   # Coverage assessment and epic validation
└── test_cases/              # Individual test case files
    ├── INDEX.md             # Test case index
    ├── TC-API-001.md        # API test cases
    ├── TC-LOAD-001.md       # YAML loading test cases
    ├── TC-PERF-001.md       # Performance test cases
    ├── TC-ERROR-001.md      # Error handling test cases
    ├── TC-DATA-001.md       # Data validation test cases
    ├── TC-SEC-001.md        # Security test cases
    └── TC-REG-001.md        # Regression test cases
```

## 🚀 Quick Start

1. **Review the test strategy**: Read [TestPlan.md](TestPlan.md) sections 1-4
2. **Browse test cases**: Check [test_cases/INDEX.md](test_cases/INDEX.md) for organized listing
