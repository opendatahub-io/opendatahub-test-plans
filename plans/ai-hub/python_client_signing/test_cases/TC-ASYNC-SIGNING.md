# Async Upload Job Signing Test Cases

**Priority**: P0
**Category**: Async Job Signing Integration
**Test Cases**: TC-009, TC-010

---

## TC-009: Async Upload Job with Model Signing Integration

- **Priority**: P0
- **Description**: End-to-end test of async upload job with integrated model signing during
     download phase
- **JIRA**: [RHOAIENG-49283](https://issues.redhat.com/browse/RHOAIENG-49283)
- **Preconditions**:
  - OpenShift cluster with RHOAI 3.4+ and TAS deployed
  - Model Registry namespace with async upload job infrastructure
  - MinIO pod with test model data
  - OCI registry pod for destination
  - TAS signing infrastructure accessible
  - Custom signing connection type mounted to job
  - Valid OIDC token available for signing
- **Test Steps**:
  1. **Setup Signing-Enabled Async Job**: Configure async upload job with model signing enabled
     (opt-out default)
  2. **Execute Async Upload with Signing**: Start async upload job that downloads model from MinIO
  3. **Verify Model Signing During Download**: Check that `model.sig` file is created in model
     root directory using OMS format
  4. **Verify Immediate Signature Verification**: Check that job immediately verifies the
     produced `model.sig`
  5. **Verify Signature Inclusion in ModelCar**: Check that `model.sig` is included in the OCI
     artifact (ModelCar)
  6. **Verify Ignore Paths Functionality**: Check that `model.sig`, `.cache`, and `.git*` files
     are excluded from signature calculation
  7. **Verify Optional Signing Configuration**: Test job with signing disabled and enabled
  8. **Verify Model Registry Integration**: Check that signed model artifact is registered correctly
- **Expected Results**:
  - Model downloaded and `model.sig` file created using OMS signing
  - Signature verified immediately without errors
  - ModelCar contains both model and signature files
  - Ignore paths work correctly for `.cache`, `.git*`, and `model.sig` files
  - Optional behavior allows enabling/disabling via configuration
  - Model Registry updated with signed artifact metadata
  - Async job completes successfully with signed model
- **Test Data**: Sample model files in MinIO (including `.cache` and `.git` directories for
     ignore testing)

---

## TC-010: Async Upload Job with Image Signing Integration

- **Priority**: P0
- **Description**: End-to-end test of async upload job with container/image signing after
     ModelCar creation and upload
- **JIRA**: [RHOAIENG-49284](https://issues.redhat.com/browse/RHOAIENG-49284)
- **Preconditions**:
  - OpenShift cluster with RHOAI 3.4+ and TAS deployed
  - Model Registry namespace with async upload job infrastructure
  - MinIO pod with test model data
  - OCI registry pod for destination with signing support
  - TAS signing infrastructure accessible
  - Custom signing connection type mounted to job
  - Valid OIDC token available for container signing
- **Test Steps**:
  1. **Setup Image Signing-Enabled Async Job**: Configure async upload job with image signing
     enabled (opt-out default)
  2. **Execute Async Upload with Image Signing**: Start async upload job that processes model
     through full pipeline
  3. **Verify ModelCar Creation and Upload**: Check that ModelCar (OCI artifact) is successfully
     created and uploaded to OCI registry
  4. **Verify Container Image Signing**: Check that `signer.sign_image(image_ref)` is called
     after OCI upload
  5. **Verify Immediate Image Signature Verification**: Check that job immediately verifies the
     produced container signature
  6. **Verify Signature Storage in Registry**: Check that container signature is stored alongside
     the ModelCar
  7. **Verify Optional Image Signing Configuration**: Test job with image signing disabled and
     enabled
  8. **Verify Model Registry Integration with Signed Images**: Check that Model Registry artifact
     URI points to signed image
  9. **Verify Combined Model and Image Signing**: Test job with both model signing and image
     signing enabled
- **Expected Results**:
  - ModelCar created and uploaded successfully
  - Container signature created after OCI upload using cosign
  - Immediate container signature verification succeeds
  - Signature stored in OCI registry alongside ModelCar
  - Optional behavior allows enabling/disabling image signing independently
  - Model Registry updated with signed container reference
  - Both model and image signing work together when enabled
  - Async job completes successfully with signed container
- **Test Data**: Sample model files in MinIO for ModelCar creation
- **Environment**: OpenShift with RHOAI, TAS, and Model Registry + async upload job infrastructure

---

## Negative Test Cases (P0 Priority)

## TC-012: Async Job Signing Failure with Invalid Signing Credentials

- **Priority**: P0
- **Description**: Validate that the async upload job fails gracefully when signing is enabled
     but signing credentials are invalid or TAS infrastructure is unreachable
- **JIRA**: TBD
- **Preconditions**:
  - OpenShift cluster with RHOAI 3.4+ and TAS deployed
  - Model Registry namespace with async upload job infrastructure
  - MinIO pod with test model data
  - OCI registry pod for destination
  - Invalid or missing signing credentials mounted to job
- **Test Steps**:
  1. **Setup Async Job with Invalid Signing Credentials**: Configure async upload job with
     signing enabled but mount invalid TAS credentials (wrong Fulcio/Rekor URLs)
  2. **Execute Async Upload Job**: Start the async upload job
  3. **Verify Job Failure**: Check that the job fails due to signing errors
  4. **Verify Fail-Closed Behavior**: Confirm no unsigned ModelCar is pushed to the OCI registry
  5. **Verify Error Reporting**: Check job logs and termination message for clear signing failure
     details
  6. **Setup Async Job with Missing Signing Secret**: Configure async upload job with signing
     enabled but no signing credentials secret mounted
  7. **Verify Missing Credentials Failure**: Confirm job fails with missing credentials error
- **Expected Results**:
  - Job fails with clear error in logs indicating signing failure
  - No unsigned ModelCar pushed to OCI registry (fail-closed behavior)
  - Termination message indicates signing failure reason
  - Job logs contain actionable error messages (e.g., unreachable Fulcio/Rekor URL, invalid token)
  - Job with missing signing secret fails before attempting upload
- **Test Data**: Sample model files in MinIO, invalid TAS configuration secrets
