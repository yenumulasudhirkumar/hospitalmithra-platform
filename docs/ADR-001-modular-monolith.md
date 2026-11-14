# ADR-001: Use a Modular Monolith for HospitalMithra v2

Status: Accepted
Date: 2026-11-14

## Context

HospitalMithra needs tenant isolation, permissions, configuration releases, audit history, OPD scheduling, and future healthcare interoperability. The Candescent Forge Control architecture separates control-plane responsibilities from data-plane runtime behavior, but it does not require every logical service to be a separately deployed microservice.

The team is small, the product is at an early stage, and the first month must produce a reliable vertical slice on a single VPS.

## Decision

Implement HospitalMithra v2 as a Java 17 Spring Boot modular monolith.

The initial modules are:

- `identity`
- `organization`
- `entitlement`
- `configuration`
- `release`
- `audit`
- `opd`
- `interoperability`

Modules communicate through explicit application interfaces and domain events. They share one deployable API and one transactional database in the first milestone.

The control-plane/data-plane boundary is a logical boundary inside the application:

- Control-plane APIs manage organizations, permissions, configuration, and releases.
- Data-plane APIs resolve and consume only effective released configuration.

## Consequences

Benefits:

- One deployment and one operational surface for the first staging milestone.
- Easier transactional consistency for scheduling and configuration release.
- Lower VPS memory and maintenance cost.
- Directly demonstrates Java/Spring, security, audit, and healthcare architecture skills.
- Module boundaries leave room for later extraction when scale or ownership requires it.

Costs and safeguards:

- A code-level boundary is not the same as a network boundary; tests and review must prevent module coupling.
- A slow or faulty module can affect the process; add timeouts and observability before integrating external systems.
- Do not allow modules to access another module's tables directly without an explicit repository/application interface.

## Revisit criteria

Consider extracting a service only after measuring one of these conditions:

- independent scaling is required;
- independent deployment is required;
- a separate team owns the module;
- failure isolation is required;
- a stable external contract already exists.
