# Python Client Signing Test Cases

**Priority**: P0 (Core), P1 (Integration)
**Category**: Core Functionality & Integration
**Test Cases**: TC-001, TC-002, TC-004 to TC-006, TC-011

---

## TC-001: Sign and Verify a Model

- **Priority**: P0
- **Description**: End-to-end test of model signing and verification
- **Preconditions**:
  - Valid TAS configuration
  - OIDC token available
  - Sample model available locally
- **Test Steps**:
  1. Initialize Signer with auto-configuration
  2. Sign model: `result = signer.sign_model(model_path)`
  3. Verify signature file is created
  4. Verify model: `verify_result = signer.verify_model(model_path)`
  5. Confirm verification succeeds
- **Expected Results**:
  - SignatureResult returned with signature_path and certificate
  - Signature file exists at expected location
  - VerificationResult shows verified=True
  - Signer identity in result matches the signing identity
- **Test Data**: Sample model

---

## TC-002: Sign and Verify a Container Image

- **Priority**: P0
- **Description**: End-to-end test of container image signing and verification
- **Preconditions**:
  - Valid TAS configuration
  - OIDC token available
  - Container image pushed to registry
  - Write permissions to registry
- **Test Steps**:
  1. Initialize Signer
  2. Sign image: `result = signer.sign_image(image_ref)`
  3. Verify signature uploaded to registry
  4. Verify image: `verify_result = signer.verify_image(image_ref)`
  5. Confirm verification succeeds
- **Expected Results**:
  - SignatureResult returned
  - Signature stored in registry alongside image
  - VerificationResult shows verified=True
  - Signer identity in result matches the signing identity
- **Test Data**: Sample container image

---

## TC-004: Sign with Invalid Credentials

- **Priority**: P0
- **Description**: Test error handling when signing with invalid or missing credentials
- **Preconditions**:
  - Sample model available
  - No valid OIDC token or expired token
- **Test Steps**:
  1. Attempt to initialize Signer without credentials
  2. Attempt to sign model
- **Expected Results**:
  - Clear error message indicating missing or invalid credentials
  - Authentication error raised
  - No signature file created
  - Helpful error message suggesting how to fix
- **Test Data**: Sample model
- **Automation Coverage**: Covered by unit tests in [model-registry/clients/python/tests/test_signing.py](https://github.com/kubeflow/model-registry/blob/main/clients/python/tests/test_signing.py):
  - `test_sign_raises_if_token_file_not_found`
  - `test_init_raises_if_token_file_not_found`
  - `test_sign_raises_signing_error_on_command_failure`

---

## Integration Test Cases (P1 Priority)

## TC-005: Sign Model from JupyterHub Notebook

- **Priority**: P1
- **Description**: Sign a model from within a Jupyter notebook environment
- **Preconditions**:
  - Jupyter notebook running in JupyterHub
  - Model registry Python client installed
  - Valid OIDC token (ServiceAccount) available
  - Sample model available in notebook workspace
- **Test Steps**:
  1. Open Jupyter notebook in JupyterHub
  2. Import signing client: `from model_registry.signing import Signer`
  3. Initialize signer: `signer = Signer()`
  4. Sign model: `result = signer.sign_model('/path/to/model')`
  5. Display signature result in notebook cell output
- **Expected Results**:
  - Signer initializes successfully with auto-detected credentials
  - Model signed successfully
  - SignatureResult displayed in cell output
  - No credential leakage in notebook output
- **Test Data**: Sample model in notebook workspace
- **Environment**: JupyterHub
- **Automation Coverage**: Manual only

---

## TC-006: Sign Container Image from JupyterHub Notebook

- **Priority**: P1
- **Description**: Sign a container image from Jupyter notebook environment
- **Preconditions**:
  - Jupyter notebook running in JupyterHub
  - Registry credentials configured
  - Container image pushed to accessible registry
  - Cosign binary available
- **Test Steps**:
  1. In notebook cell, initialize signer
  2. Sign container image: `result = signer.sign_image('quay.io/org/image:tag')`
  3. Verify signature uploaded to registry
  4. Display result in notebook
- **Expected Results**:
  - Image signed successfully from notebook
  - Signature uploaded to container registry
  - Success message displayed
  - No credential leakage in notebook output
- **Test Data**: Sample container image
- **Environment**: JupyterHub with container runtime access
- **Automation Coverage**: Manual only

---

## Negative Test Cases (P0 Priority)

## TC-011: Signing Failure Scenarios

- **Priority**: P0
- **Description**: Validate that signing failures are handled gracefully across multiple failure
  modes: invalid TAS configuration, invalid registry credentials, and missing signatures
- **Preconditions**:
  - Valid TAS configuration (for baseline)
  - Sample model available locally
  - Container image pushed to registry
  - OIDC token available

### TC-011a: Sign Model with Invalid TAS Configuration

- **Test Steps**:
  1. Initialize Signer with invalid Fulcio/Rekor URLs
  2. Attempt to sign model: `result = signer.sign_model(model_path)`
- **Expected Results**:
  - Signing operation fails with clear error
  - Error message indicates TAS connectivity/configuration issue
  - No signature file created
  - No partial or corrupted signature left behind
- **Automation Coverage**: Covered by unit test in [model-registry/clients/python/tests/test_signing.py](https://github.com/kubeflow/model-registry/blob/main/clients/python/tests/test_signing.py):
  - `test_initialize_raises_initialization_error_on_command_failure`

### TC-011b: Sign Container Image with Invalid Registry Credentials

- **Test Steps**:
  1. Initialize Signer with valid TAS config but invalid registry write credentials
  2. Attempt to sign image: `result = signer.sign_image(image_ref)`
- **Expected Results**:
  - Signing operation fails with authentication/authorization error
  - Error message indicates registry permission issue
  - No signature artifact stored in registry
- **Automation Coverage**: Covered by E2E test in [opendatahub-tests](https://github.com/opendatahub-io/opendatahub-tests/blob/main/tests/model_registry/model_registry/python_client/signing/test_signing_infrastructure.py):
  - `test_sign_image_invalid_registry_credentials`

### TC-011c: Verify Model with Missing Signature File

- **Test Steps**:
  1. Prepare model directory without any `model.sig` file
  2. Attempt to verify model: `verify_result = signer.verify_model(model_path)`
- **Expected Results**:
  - Verification fails with clear error
  - Error message indicates missing signature
  - `verified=False` returned
- **Automation Coverage**: Covered by E2E test in [opendatahub-tests](https://github.com/opendatahub-io/opendatahub-tests/blob/main/tests/model_registry/model_registry/python_client/signing/test_signing_infrastructure.py):
  - `test_verify_model_missing_signature`

- **Test Data**: Sample model, sample container image, invalid TAS configurations

---

## Change Log

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-03-03 | Initial test cases for Python client signing functionality |
| 1.1 | 2026-03-20 | Removed TC-003 (Cross-Identity Verification) — signer identity validation merged into TC-001/TC-002 expected results. Removed TC-011b (Verify Tampered Model) as low value. Re-lettered remaining TC-011 sub-cases. Added automation coverage notes for TC-004, TC-005, TC-006, TC-011a, TC-011b, TC-011c. Marked TC-005 and TC-006 as manual only. |
