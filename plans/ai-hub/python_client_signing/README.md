# Python Client Signing Functionality

Model and container image signing using the Model Registry Python client with Trusted Artifact
Signer (TAS).

## Quick Start

### 0. Prerequisites

- **OpenShift Cluster**: 4.19+ with RHOAI 3.4+
- **TAS Instance**: Securesign deployed in `trusted-artifact-signer` namespace
  - See [TAS Setup Guide](https://github.com/jonburdo/model-signing-dev/tree/main/testing) for
    installation
- **Python Client**: Latest model-registry package from PyPI

### 1. Install Python Client

```bash
# Install from PyPI
pip install model-registry

# Or from source
pip install git+https://github.com/kubeflow/model-registry.git
```

### 2. Sign a Model

```bash
# Set up environment (auto-discovered if running in cluster)
export OIDC_ISSUER=$(oc whoami --show-server)/.well-known/openid-configuration

# Sign a model
python -c "
from model_registry.signing import ModelSigner

signer = ModelSigner()
result = signer.sign_model('path/to/model')
"
```

### 3. Verify a Model

```bash
# Verify the signed model
python -c "
from model_registry.signing import ModelSigner

signer = Signer()
result = signer.verify_model('path/to/model')
"
```

## Usage Examples

### Sign a Container Image

```python
from model_registry.signing import ModelSigner

# Initialize signer (auto-configures from environment)
signer = ModelSigner()

# Sign container image
image_ref = "quay.io/myorg/myimage:latest"
result = signer.sign_image(image_ref)

```

### Verify a Container Image

```python
from model_registry.signing import Signer

signer = Signer()

# Verify image signature
image_ref = "quay.io/myorg/myimage:latest"
result = signer.verify_image(image_ref)
```

### Manual Configuration

Details can be found: <https://github.com/jonburdo/model-signing-dev/blob/main/model-sign>

```python
from model_registry.signing import ModelSigner

config = {
    "tuf_url": os.getenv("TUF_URL"),
    "fulcio_url": os.getenv("FULCIO_URL"),
    "rekor_url": os.getenv("REKOR_URL"),
    "tsa_url": os.getenv("TSA_URL"),
    "oidc_issuer": os.getenv("OIDC_ISSUER"),
    "root": os.getenv("ROOT_URL"),
    "root_checksum": os.getenv("ROOT_CHECKSUM"),
}
signer = ModelSigner(**config)
result = signer.sign_model('path/to/model')
```

## Configuration

### Environment Variables

The signer auto-configures from environment variables:

| Variable | Description | Default |
|----------|-------------|---------|
| `SIGNING_FULCIO_URL` | Fulcio CA URL | Auto-discovered from cluster |
| `SIGNING_REKOR_URL` | Rekor transparency log URL | Auto-discovered from cluster |
| `SIGNING_TUF_URL` | TUF metadata URL | Auto-discovered from cluster |
| `SIGNING_TSA_URL` | Timestamp Authority URL | Auto-discovered from cluster |

### Discover OIDC Issuer

```bash
# Get OIDC issuer from cluster
APISERVER=$(oc whoami --show-server)
TOKEN=$(oc whoami -t)
OIDC_ISSUER=$(curl -sS -H "Authorization: Bearer $TOKEN" \
  "$APISERVER/.well-known/openid-configuration" | jq -r '.issuer')

echo "OIDC Issuer: $OIDC_ISSUER"
```

## Testing

Run the test suite:

```bash
# Run all tests
pytest tests/signing/

# Run specific test priority
pytest -m p0 tests/signing/

# Run with coverage
pytest --cov=model_registry.signing tests/signing/
```

## References

- **Epic**: [RHOAIENG-45133](https://issues.redhat.com/browse/RHOAIENG-45133)
- **Parent Feature**: [RHAISTRAT-1074](https://issues.redhat.com/browse/RHAISTRAT-1074)
- **TAS Setup**: [model-signing-dev/testing](<https://github.com/jonburdo/model-signing-dev/tree>
  /main/testing)
- **Sigstore Documentation**: <https://docs.sigstore.dev/>
- **Cosign Documentation**: <https://docs.sigstore.dev/cosign/overview/>
