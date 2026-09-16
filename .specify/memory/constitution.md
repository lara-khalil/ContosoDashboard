<!--
Sync Impact Report
- Version change: 0.0.0 → 1.0.0
- Modified principles: N/A (new governance document)
- Added sections: Additional Constraints, Development Workflow
- Removed sections: N/A
- Deferred items: TODO(RATIFICATION_DATE): confirm the original adoption date for this constitution.
-->

# ContosoDashboard Constitution

## Core Principles

### I. Security-by-Design for Training
ContosoDashboard MUST be developed as a safe training environment, not a production system. All user-facing features, authentication behavior, and data access must preserve user isolation, restrict access by role, and avoid exposing production-grade secrets, external dependencies, or unsafe defaults.

This rule is non-negotiable because the repository is explicitly a learning project. If a feature introduces a security boundary or user-scoped data path, the implementation must enforce authorization at the service layer and the UI boundary before it is considered complete.

### II. User-Centered Workflow Integrity
Every task, project, and notification flow MUST align with the real user journey for a dashboard application: clear visibility, consistent status transitions, role-relevant actions, and explicit ownership of work.

The project exists to teach disciplined product behavior, so each feature must be understandable to a learner and traceable to a business outcome. Any screen or service that changes task state, project membership, or user visibility must keep the workflow coherent and auditable.

### III. Test-First Verification
No feature, fix, or behavior change is complete until the relevant validation has been executed and the evidence is recorded in the working branch. For user-facing behavior or security-sensitive changes, a failing check or targeted repro MUST exist before implementation, followed by a passing validation after the fix.

This principle prevents speculative changes and keeps the repository aligned with Spec-Driven Development. When a change cannot be verified by existing tests or a targeted validation step, it is considered incomplete.

### IV. Data Integrity and Access Boundaries
The data model MUST preserve ownership, membership, and task constraints. Services MUST reject unauthorized reads or writes, prevent insecure direct object references, and enforce the same rules whether data is accessed through a page, component, or API-like service call.

The rationale is straightforward: a dashboard that appears to work but allows cross-user access is not acceptable. Data boundaries are part of the product contract and must be treated as mandatory behavior, not optional checks.

### V. Simplicity, Clarity, and Maintainability
The solution MUST favor small, clear abstractions, explicit naming, and straightforward service boundaries over broad frameworks or hidden logic. Features must remain understandable to students and maintainers without requiring deep knowledge of incidental implementation details.

Complexity is allowed only when it is required by the problem and is documented. The default posture is simplicity: keep code obvious, local, and easy to verify.

## Additional Constraints
ContosoDashboard is a training-focused application that MUST remain offline-first, local-only, and intentionally limited to educational scenarios. The project MUST use the existing ASP.NET Core and Blazor Server patterns unless a deliberate governance change is approved.

The repository MUST not present itself as production-ready infrastructure. Authentication, data storage, and deployment assumptions must remain clearly labeled as mock or training-oriented. Any cloud migration path must be described as an explicit future step, not a silent requirement.

## Development Workflow
All work MUST begin from a clearly defined requirement or specification. Changes to behavior, security rules, or user workflows MUST be reflected in the relevant spec artifacts before implementation proceeds.

Reviews MUST confirm that requirements are met, security boundaries remain intact, and validation evidence is present. Pull requests that alter task logic, permissions, or project access must explicitly explain the impact on users and the verification performed.

## Governance
This constitution supersedes informal project practices and governs design, implementation, review, and change management for ContosoDashboard. All contributors and reviewers MUST operate under these principles unless a formal amendment is approved.

Amendments MUST be documented in writing, include the rationale for the change, and identify the impacted principles or workflows. Major changes to required security behavior, role boundaries, or project scope require explicit review and approval before they are adopted. Minor wording or clarification updates may be approved with narrower review if they do not change the underlying obligations.

Compliance review is required before merge for any change that affects security, authorization, architecture, or training scope. The review must confirm that the change aligns with the constitution and that the relevant validation evidence exists. Repeated non-compliance is treated as a governance issue and must be addressed through corrective action, not by over-riding the written policy.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): confirm the original adoption date for this constitution. | **Last Amended**: 2026-09-16
