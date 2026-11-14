# ADR-002: Resolve Tenant Context Before Business Access

Status: Accepted
Date: 2026-11-14

## Decision

Every protected request must establish an authenticated subject and an explicit tenant context before loading business data.

The request context will eventually contain:

```text
subjectId
organizationId
branchId
productOrModule
environment
roles
permissions
correlationId
```

The backend must validate, in order:

1. The caller identity.
2. Organization membership.
3. Product/module entitlement.
4. Environment and branch access.
5. Role and permission.
6. Ownership or authorized relationship to the requested resource.

Changing an organization ID, patient ID, appointment ID, or branch ID in a browser request must never bypass these checks.

The first implementation will expose an OIDC integration boundary and use a local test identity provider. It will not create a custom password authentication system.

## Consequences

- Tenant isolation becomes a backend invariant rather than a UI convention.
- Integration tests must include negative cross-tenant cases.
- The identity provider can change without rewriting business modules.
- Real patient data remains out of local development and staging.
