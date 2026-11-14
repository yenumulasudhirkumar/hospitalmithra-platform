# Security Policy

## Development rules

- Use synthetic patients and synthetic organizations only.
- Never commit `.env` files, passwords, API keys, SSH keys, database dumps, or authentication cookies.
- Do not place patient content in logs, test output, GitHub issues, or AI prompts.
- Every protected resource must be authorized by authenticated subject, organization, product/module, environment, role, and resource relationship.
- The UI is not a security boundary; backend authorization is mandatory.
- Draft configuration must never be returned by runtime APIs.
- Audit events must avoid unnecessary sensitive content and must be append-only.

## Reporting a vulnerability

Do not open a public issue for a suspected security vulnerability. Report it privately to the repository owner with reproduction steps and impact. Do not include real patient data in the report.

## Deployment rule

Staging and production credentials must be stored outside GitHub source files. VPS access must use the dedicated non-root deployment account and least-privilege keys described in the roadmap runbook.
