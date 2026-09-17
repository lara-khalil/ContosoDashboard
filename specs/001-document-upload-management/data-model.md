# Data Model: Document Upload and Management

## Entity overview

### Document
Represents a stored work file and its metadata.

| Field | Type | Notes |
|---|---|---|
| DocumentId | int | Primary key |
| Title | string | Required |
| Description | string? | Optional |
| Category | string | Required, uses predefined category values |
| Tags | string? | Optional comma-delimited or normalized text |
| FileName | string | Original file name for display only |
| StoredFileName | string | Generated secure storage name |
| FilePath | string | Internal storage path, not user-supplied |
| FileType | string | Up to 255 characters |
| FileSizeBytes | long | Original file size |
| UploadDateUtc | DateTime | Required |
| UploadedByUserId | int | Required, references User |
| ProjectId | int? | Optional project association |
| TaskId | int? | Optional task association |
| IsDeleted | bool | Soft-delete safety for recovery and referential integrity |

Relationships:
- Many-to-one with User (`UploadedByUserId`)
- Many-to-one with Project (`ProjectId`)
- Many-to-one with Task (`TaskId`)
- One-to-many with DocumentShare
- One-to-many with DocumentActivity

### DocumentShare
Represents an explicit share permission from a document to a user or team.

| Field | Type | Notes |
|---|---|---|
| DocumentShareId | int | Primary key |
| DocumentId | int | Required |
| RecipientUserId | int? | Optional if team-level sharing is represented later |
| TeamName | string? | Optional shared team reference |
| SharedByUserId | int | Required |
| SharedDateUtc | DateTime | Required |
| IsActive | bool | Revoked shares become inactive |

Relationships:
- Many-to-one with Document
- Many-to-one with User (`SharedByUserId`)
- Many-to-one with User (`RecipientUserId`)

### DocumentActivity
Tracks evidence for audit and compliance reporting.

| Field | Type | Notes |
|---|---|---|
| ActivityId | int | Primary key |
| DocumentId | int? | Nullable for system-level actions |
| ActorUserId | int | Required |
| ActionType | string | Upload, download, delete, share, preview |
| ActionDateUtc | DateTime | Required |
| Details | string? | Additional context and metadata |

Relationships:
- Many-to-one with User (`ActorUserId`)
- Many-to-one with Document (`DocumentId`)

### ProjectDocumentAssociation
A lightweight join model if project-level sharing is modeled as a derived relationship rather than a new table.

| Field | Type | Notes |
|---|---|---|
| ProjectId | int | Required |
| DocumentId | int | Required |

This may be represented by the document’s `ProjectId` alone for the first implementation; the model stays flexible if project document listing needs explicit join records later.

### TaskDocumentAssociation
A task-to-document relationship, constrained to the task’s project.

| Field | Type | Notes |
|---|---|---|
| TaskId | int | Required |
| DocumentId | int | Required |

This can be represented by a direct `TaskId` on the document or a link entity depending on how task attachments are implemented in the UI.

## Validation rules

- Title is required and should be non-empty.
- Category must match the allowed predefined values.
- File size must be greater than 0 and less than or equal to 25 MB.
- File type must be a supported value and stored in a safe length-constrained field.
- Storage path must be generated server-side and not derived from the client’s original file name.
- Project association must be validated against current project membership or management permissions.
- Task association must be restricted to a task in the same project as the document when both are present.
- Sharing must enforce access checks before any recipient sees the document.

## State transitions

### Document lifecycle
- Draft/queued: user selected file and metadata entry started
- Validated: file type, size, and security checks passed
- Stored: metadata record exists and file is persisted
- Accessible: document is visible to allowed users
- Updated: metadata or file replaced
- Deleted: record and file are removed
- Revoked/hidden: share permission or project membership no longer grants access

### Share lifecycle
- Active while the recipient remains allowed and the share is not revoked
- Inactive after access is removed or the document is deleted

## Notes for implementation

The design intentionally keeps the initial model in the existing EF Core patterns used by the repository, without creating a separate microservice or distributed file system.
