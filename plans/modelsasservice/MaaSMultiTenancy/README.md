# MaaS Multi-Tenancy — Per-Tenant Gateway and Identity Isolation

Operator-managed multi-tenancy for MaaS enabling platform admins to provision isolated organizational tenants via a single CR, with automated namespace, gateway, identity realm, and policy lifecycle management.

## Links

- **Jira RFE**: [RHAIRFE-1487](https://redhat.atlassian.net/browse/RHAIRFE-1487)
- **PoC Repository**: [bartoszmajsak/maas-multi-tenancy-poc](https://github.com/bartoszmajsak/maas-multi-tenancy-poc/tree/maas-cr-odh-operator-integration)
- **Dependency**: [RHAISTRAT-1120](https://redhat.atlassian.net/browse/RHAISTRAT-1120) — MaaS External OIDC Support

## Test Plan

- [TestPlan.md](TestPlan.md) — Full test plan for multi-tenancy feature validation

## Test Automation

Automated tests will be implemented in the RHOAI collection test repository, covering tenant provisioning, isolation enforcement, API key management, BYOIDP flows, and shared model access grants.
