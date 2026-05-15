# Python Client Signing Functionality Test Plan

**Model Registry Team - Python Client Signing**

## Document Information

- **Feature**: Python Client Signing Functionality
- **Epic**: [RHOAIENG-45133](https://issues.redhat.com/browse/RHOAIENG-45133)
- **Parent Feature**: [RHAISTRAT-1074](https://issues.redhat.com/browse/RHAISTRAT-1074) - Create
  ability to sign and verify AI Artifacts in Registry
- **Team**: Model Registry / AI Hub
- **Version**: 1.0
- **Last Updated**: 2026-03-03

---

## 1. Executive Summary

### 1.1 Purpose

This test plan defines the testing approach for the **Python Client Signing Functionality**
delivered by the Model Registry team. The Python client provides APIs for signing and verifying AI
artifacts (models, datasets) and container images using industry-standard signing tools.

### 1.2 Scope

#### In Scope (Python Client Signing Responsibilities)

- **Positive Flow Testing - Models**
  - Successful signing of AI models
  - Successful verification of signed models
- **Positive Flow Testing - Container Images**
  - Successful signing of OCI container images
  - Successful verification of signed container images
- **Negative Flow Testing**
  - Invalid credentials (authentication failures)
  - Cross-identity verification (verifying signer identity)
  - Tampered signatures (signature integrity validation)

#### Out of Scope (Other Teams/Components)

- **UI Integration** - Dashboard or UI components for signing
- **Performance Testing** - Load testing, scalability, performance benchmarks
- **TAS Functionality Verification** - Testing Fulcio, Rekor, or TAS infrastructure itself
- **Async Signing Job** - Background job for asynchronous signing (separate epic)
- **Policy Enforcement** - Admission control or policy-based verification
- **Certificate Management** - Certificate lifecycle and rotation (TAS responsibility)
- **Multi-Version Compatibility** - Testing across multiple Python versions or client versions

### 1.3 Test Objectives

1. Validate model signing and verification (positive flow)
2. Validate container image signing and verification (positive flow)
3. Validate error handling and negative scenarios (invalid credentials, cross-identity verification,
   tampered signatures)

---

## 2. Test Strategy

### 2.1 Test Levels

- **Unit Testing** - Individual signing/verification functions, configuration parsing
- **Integration Testing** - End-to-end signing with TAS services, registry upload
- **API Testing** - Python API interface, data models, error handling

### 2.2 Test Types

- **Positive Testing** - Valid signing/verification workflows, correct configurations
- **Negative Testing** - Invalid inputs, authentication failures, malformed signatures

---

## 3. Test Environment

### 3.1 Test Cluster Configuration

- **OpenShift**: Version 4.19+
- **RHOAI**: Version 3.4+ (with Model Registry components)
- **Python Client**: Latest model-registry Python client from PyPI
- **Notebook Environment**: JupyterHub
- **TAS (Trusted Artifact Signer)**:
  - RHTAS Operator installed
  - Securesign instance deployed in `trusted-artifact-signer` namespace
  - Dynamic OIDC issuer from OpenShift cluster
  - External routes enabled for Fulcio, Rekor services

### 3.2 Test Data Requirements

#### Artifact Types

- **Models** - AI models in various formats
- **Container Images** - OCI container images

#### TAS Configuration

**Setup Reference**: [model-signing-dev/testing](https://github.com/jonburdo/model-signing-dev/tree/main/testing)

The TAS instance is deployed using the RHTAS operator with dynamic configuration:

```bash
# OIDC Issuer discovered from cluster
APISERVER=$(oc whoami --show-server)
TOKEN=$(oc whoami -t)
OIDC_ISSUER=$(curl -sS -H "Authorization: Bearer $TOKEN" \
  "$APISERVER/.well-known/openid-configuration" | jq -r '.issuer')

# Example OIDC issuers (cluster-dependent):
# - ROSA: https://rh-oidc.s3.us-east-1.amazonaws.com/<cluster-id>
# - K8s 1.20+: https://kubernetes.default.svc

# Securesign instance configuration
NAMESPACE: trusted-artifact-signer
EXTERNAL_ACCESS: true
ORGANIZATION_NAME: RHOAI
ORGANIZATION_EMAIL: admin@example.com
```

Services are accessed via external routes when `EXTERNAL_ACCESS=true`

#### Service Account Configuration

```yaml
# OpenShift ServiceAccount with projected token
apiVersion: v1
kind: ServiceAccount
metadata:
  name: model-signer
  annotations:
    serviceaccounts.openshift.io/oauth-redirectreference.tas: '{"kind":"OAuthRedirectReference","apiVersion":"v1","reference":{"kind":"Route","name":"tas"}}'
```

### 3.3 Test Users

- **ServiceAccount** - OpenShift service account with OIDC token
- **User with OIDC Token** - Regular user with valid OIDC credentials
- **Anonymous User** - Testing error handling without credentials

---

## 4. API Methods Under Test

### 4.1 Unified Signer Class

| Method | Purpose | Priority |
|--------|---------|----------|
| `Signer.__init__()` | Initialize signer with auto-configuration | P0 |
| `Signer.sign_model(artifact_path)` | Sign OMS artifact (model/dataset) | P0 |
| `Signer.verify_model(artifact_path)` | Verify OMS artifact signature | P0 |
| `Signer.sign_image(image_ref)` | Sign container image with cosign | P0 |
| `Signer.verify_image(image_ref)` | Verify container image signature | P0 |

### 4.2 Configuration Interface

| Method | Purpose | Priority |
|--------|---------|----------|
| `Config.from_environment()` | Auto-configure from env vars | P1 |
| `Config.from_service_account()` | Load config from SA token | P1 |
| `Config.validate()` | Validate configuration | P1 |

---

## 5. Test Cases

### 5.1 Core Functionality Tests

| Test Case | Description | Priority |
|-----------|-------------|----------|
| TC-001 | Sign and verify a model | P0 |
| TC-002 | Sign and verify a container image | P0 |
| TC-004 | Sign with invalid credentials | P0 |
| TC-005 | Sign model from JupyterHub notebook | P1 |
| TC-006 | Sign container image from JupyterHub notebook | P1 |
| TC-009 | Async upload job with model signing integration | P0 |
| TC-010 | Async upload job with image signing integration | P0 |
| TC-011 | Signing failure scenarios (invalid config, tampered model, missing signature) | P0 |
| TC-012 | Async job signing failure with invalid signing credentials | P0 |

**Total**: 9 test cases (P0: 7, P1: 2)

---

### 5.2 Test Case Details

Test case details are organized in separate files under the `test_cases/` directory:

#### Core & Integration Tests

**File**: [test_cases/TC-CORE-SIGNING.md](./test_cases/TC-CORE-SIGNING.md)

**Core Functionality (P0 Priority):**

- **TC-001**: Sign and Verify a Model
- **TC-002**: Sign and Verify a Container Image
- **TC-004**: Sign with Invalid Credentials

**Integration Tests (P1 Priority):**

- **TC-005**: Sign Model from JupyterHub Notebook
- **TC-006**: Sign Container Image from JupyterHub Notebook

#### Negative Tests (P0 Priority)

**File**: [test_cases/TC-CORE-SIGNING.md](./test_cases/TC-CORE-SIGNING.md)

- **TC-011**: Signing Failure Scenarios

#### Async Job Signing (P0 Priority)

**File**: [test_cases/TC-ASYNC-SIGNING.md](./test_cases/TC-ASYNC-SIGNING.md)

- **TC-009**: Async Upload Job with Model Signing Integration
- **TC-010**: Async Upload Job with Image Signing Integration
- **TC-012**: Async Job Signing Failure with Invalid Signing Credentials

**Index**: See [test_cases/INDEX.md](./test_cases/INDEX.md) for a complete overview of all test cases.

---

## 6. Test Data Management

### 6.1 Artifact Repository

- **Location**: Shared storage or S3 bucket
- **Contents**:
  - Sample models
  - Container images
  - Pre-signed artifacts for verification testing

### 6.2 Signature Archive

- **Location**: Test data repository
- **Contents**:
  - Valid signatures from previous versions
  - Invalid/tampered signatures
  - Signatures from different TAS instances
  - Revoked certificate signatures

### 6.3 Configuration Templates

- **Public Sigstore Config**
- **Private TAS Config**
- **ServiceAccount Configurations**
- **OIDC Token Samples** (sanitized for testing)

---

## 7. Document Change Log

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2026-03-03 | QA Team | Initial test plan for Python client signing functionality |

---

**End of Test Plan**
