# HospitalMithra Platform

HospitalMithra v2 healthcare platform foundation with tenant isolation, released configuration, OPD workflows, audit, and FHIR integration boundaries.

## Current status

This repository contains the initial Spring Boot API skeleton. The first implementation milestone is a Forge-Control-style modular monolith:

```text
control plane -> organizations, permissions, configuration, releases, audit
data plane    -> runtime APIs using only effective released configuration
```

No patient data, production credentials, or live hospital integrations belong in this repository.

## Prerequisites

- Java 17
- Maven 3.8+

## Run locally

```bash
mvn spring-boot:run
```

Check readiness:

```bash
curl http://localhost:8080/actuator/health
```

## Verify

```bash
mvn verify
```

## Repository structure

```text
src/main/java/com/hospitalmithra/platform/
  audit/             # append-only audit boundary
  configuration/     # effective configuration resolution boundary
  entitlement/       # product/module access boundary
  identity/          # OIDC and authenticated subject boundary
  interoperability/  # FHIR adapter boundary
  opd/               # appointment and queue domain
  organization/      # tenant, branch, and workspace domain
  release/           # draft/current/previous release lifecycle
  shared/            # deliberately small cross-module primitives
```

See `docs/` for architecture decisions and `SECURITY.md` for development security rules.

## Deployment rule

Staging deployment will use an immutable Docker image built by GitHub Actions and pulled by the VPS. Do not build uncommitted source code directly on the VPS.
