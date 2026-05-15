# Python Client Signing Test Cases Index

This directory contains test cases for the Python Client Signing functionality.

## Test Case Files

### Core & Integration Tests

**File**: [TC-CORE-SIGNING.md](./TC-CORE-SIGNING.md)

**Core Functionality (P0 Priority):**

- **TC-001**: Sign and Verify a Model
- **TC-002**: Sign and Verify a Container Image
- **TC-004**: Sign with Invalid Credentials

**Negative Tests (P0 Priority):**

- **TC-011**: Signing Failure Scenarios (invalid TAS config, tampered model, invalid registry
  credentials, missing signature)

**Integration Tests (P1 Priority):**

- **TC-005**: Sign Model from JupyterHub Notebook
- **TC-006**: Sign Container Image from JupyterHub Notebook

### Async Job Signing (P0 Priority)

**File**: [TC-ASYNC-SIGNING.md](./TC-ASYNC-SIGNING.md)

- **TC-009**: Async Upload Job with Model Signing Integration
- **TC-010**: Async Upload Job with Image Signing Integration
- **TC-012**: Async Job Signing Failure with Invalid Signing Credentials

## Test Summary

| Priority | Test Cases | File |
|----------|------------|------|
| P0 | TC-001, TC-002, TC-004, TC-011 | TC-CORE-SIGNING.md |
| P0 | TC-009, TC-010, TC-012 | TC-ASYNC-SIGNING.md |
| P1 | TC-005 to TC-006 | TC-CORE-SIGNING.md |

**Total Test Cases**: 9 (7 P0, 2 P1)
