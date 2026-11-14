# ADR-003: Defer Database Commitment Until the v1 Audit

Status: Accepted
Date: 2026-11-14

## Decision

The application will use Flyway migrations and repository interfaces from the first commit. The concrete database will be selected after a short audit of the existing v1 schema:

- Use PostgreSQL for a greenfield v2 with no immediate data migration.
- Use MySQL 8 if v2 must reuse existing HospitalMithra data immediately.

No database rewrite will be performed only for technology preference.

## Required checks before choosing

- Existing v1 schema and migration quality.
- Data that must be retained or migrated.
- Transaction and locking requirements for slot allocation.
- Local and VPS operational familiarity.
- Backup and restore tooling.
- Availability of a clean test database in CI.

## Constraints

- No real patient data in the development repository or CI.
- No public database port on the VPS.
- Every schema change must be versioned and tested from an empty database.
- A backup is not considered valid until a restore test succeeds.
