# Research: Document Upload and Management

## Decision

The feature will use the existing Blazor Server + EF Core SQLite architecture and add a document domain with secure storage, authorization checks, and audit logging built into the existing service layer.

## Rationale

The repository already provides role-based auth, service-layer security patterns, and a relational data model. Reusing those conventions reduces implementation risk and keeps the feature understandable in the training context. Local disk storage matches the project’s offline-first requirement and supports a future cloud abstraction without changing business workflows.

## Key Findings

### 1. Storage and access pattern
- Store metadata in SQLite so document records can be searched, filtered, and scaled with the existing ApplicationDbContext approach.
- Store actual file bytes on the local filesystem in a non-public directory, using a generated unique path rather than user-controlled file names.
- Provide a storage abstraction layer so the local mechanism can be swapped for cloud storage later without changing document use cases.

### 2. Authorization model
- Reuse the existing mock authentication and role claims (`Employee`, `TeamLead`, `ProjectManager`, `Administrator`).
- Enforce document permissions in service methods and page-level authorization boundaries.
- Project membership, project management role, document ownership, and team-based sharing remain the main permission drivers.

### 3. Upload and validation workflow
- Accept uploads via one action that supports multiple files and validates each file individually.
- Enforce file type and size checks before storing metadata or file data.
- Treat malware scanning as a validation gate in the service; in a training app this can be modeled as a checked security service with a fail-fast outcome.

### 4. Search and dashboard experience
- Use server-side filtering and search against existing document metadata fields.
- Keep recent document widget logic scoped to the current user and show an empty state when none exist.
- Keep list browsing usable for 500 accessible documents with simple database queries and pagination or limiting patterns.

## Alternatives considered

### Local-only document storage without abstraction
- Rejected because the spec explicitly requires a future storage swap path without changing business rules.

### Overloading existing task/project models
- Rejected because document lifecycles, access rules, and audit events are distinct enough to warrant dedicated entities.

### Requiring external cloud file storage
- Rejected because the feature must work offline and in the training environment without external services.

## Open design constraints resolved

- The feature will remain internal to the web app and not introduce independent microservices or external APIs.
- The design assumes mock authentication claims already carry role and department context needed for sharing.
- Audit records will be kept as explicit data entities and used for reporting rather than being inferred only from logs.
