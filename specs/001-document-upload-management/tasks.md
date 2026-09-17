# Tasks: Document Upload and Management

**Input**: Design documents from `/specs/001-document-upload-management/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Prepare the existing Blazor Server app for document storage, validation, and metadata workflows.

- [ ] T001 Create the document feature structure and storage conventions in ContosoDashboard/Models/, ContosoDashboard/Services/, ContosoDashboard/Pages/, and the feature spec folder
- [ ] T002 [P] Configure local document storage and security settings in ContosoDashboard/Program.cs and appsettings files
- [ ] T003 [P] Add the document database metadata and service registration updates in ContosoDashboard/Data/ApplicationDbContext.cs and ContosoDashboard/Program.cs

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Establish the shared document foundation that all user stories depend on.

**Critical checkpoint**: No user story work can begin until the foundation is complete.

- [ ] T004 Create the base document entity model in ContosoDashboard/Models/Document.cs with required metadata fields, category values, and storage metadata
- [ ] T005 [P] Create the share/audit models in ContosoDashboard/Models/DocumentShare.cs and ContosoDashboard/Models/DocumentActivity.cs
- [ ] T006 [P] Extend ContosoDashboard/Data/ApplicationDbContext.cs with DbSet entries, indexes, and validation constraints for document-related entities
- [ ] T007 Implement the local file storage abstraction and secure filename generation in ContosoDashboard/Services/IFileStorageService.cs and ContosoDashboard/Services/LocalFileStorageService.cs
- [ ] T008 Implement document validation rules for file type, size, project association, and security checks in ContosoDashboard/Services/DocumentValidationService.cs
- [ ] T009 Add role-aware authorization helpers for document access, ownership, project membership, and admin reporting in ContosoDashboard/Services/DocumentAuthorizationService.cs
- [ ] T010 Add audit logging and activity creation helpers in ContosoDashboard/Services/DocumentActivityService.cs

**Checkpoint**: Foundation ready - all story implementations may begin in parallel.

---

## Phase 3: User Story 1 - Upload and Organize Work Documents (Priority: P1)

**Goal**: Let employees upload supported documents with metadata, validate each file, and keep them organized under their assigned project or personal workspace.

**Independent Test**: An authenticated employee can upload a supported file with a title and category, see progress and completion feedback, and find the resulting document in the personal document list.

### Implementation for User Story 1

- [ ] T011 [US1] Implement the main document service logic in ContosoDashboard/Services/DocumentService.cs for create, validate, store, and list operations
- [ ] T012 [US1] Add the upload workflow UI in ContosoDashboard/Pages/Documents.razor or the app’s equivalent document upload page
- [ ] T013 [US1] Add per-file validation, rejection messages, and incomplete-record prevention in ContosoDashboard/Services/DocumentService.cs and the upload page flow
- [ ] T014 [US1] Connect project association logic to the existing project and user rules in ContosoDashboard/Services/DocumentService.cs and ContosoDashboard/Services/ProjectService.cs
- [ ] T015 [US1] Add upload success/error notifications and document list display metadata in ContosoDashboard/Pages/Documents.razor and ContosoDashboard/Services/DocumentService.cs

**Checkpoint**: At this point, User Story 1 should be fully functional and independently testable.

---

## Phase 4: User Story 2 - Find, Preview, and Use Accessible Documents (Priority: P1)

**Goal**: Let users search, filter, sort, preview, and download documents they are allowed to access.

**Independent Test**: A user with personal, project, and shared documents can search and filter the list, preview a supported PDF/image, and download an accessible document while inaccessible records remain hidden.

### Implementation for User Story 2

- [ ] T016 [US2] Implement document query logic for list, sort, filter, and search in ContosoDashboard/Services/DocumentService.cs
- [ ] T017 [US2] Add the My Documents view and accessible document list rendering in ContosoDashboard/Pages/Documents.razor or a dedicated document listing page
- [ ] T018 [US2] Add search, category, project, and date-range filtering controls in the document list UI and service layer
- [ ] T019 [US2] Add browser preview and file download actions with permission checks in ContosoDashboard/Services/DocumentService.cs and the document page workflow
- [ ] T020 [US2] Enforce hidden/inaccessible document behavior in both list queries and direct access routes to prevent IDOR exposure

**Checkpoint**: At this point, User Stories 1 and 2 should both work independently.

---

## Phase 5: User Story 3 - Manage and Share Documents Safely (Priority: P1)

**Goal**: Allow document owners and authorized managers to update metadata, replace files, delete records, and share documents with selected users or teams.

**Independent Test**: An owner can edit metadata, replace a file, share it, and confirm deletion; unauthorized users cannot complete those actions.

### Implementation for User Story 3

- [ ] T021 [US3] Implement metadata edit and file replacement logic in ContosoDashboard/Services/DocumentService.cs
- [ ] T022 [US3] Add document management actions and confirmation prompts in ContosoDashboard/Pages/Documents.razor or document detail page components
- [ ] T023 [US3] Add document deletion flow that removes both the metadata record and stored file in ContosoDashboard/Services/DocumentService.cs and the UI action handlers
- [ ] T024 [US3] Implement document sharing logic and share recipient validation in ContosoDashboard/Services/DocumentShareService.cs and ContosoDashboard/Services/DocumentService.cs
- [ ] T025 [US3] Add in-app notification delivery for recipients and ensure share visibility in shared documents views
- [ ] T026 [US3] Enforce project manager and owner permission checks for edit, replace, delete, and share actions in the service layer

**Checkpoint**: User Story 3 is independently functional and safe from unauthorized access.

---

## Phase 6: User Story 4 - Work with Project and Task Documents (Priority: P2)

**Goal**: Surface project and task document context inside the areas where users already work.

**Independent Test**: A project team member can view project documents and a user viewing a task can attach or upload a related document that is associated with the task’s project.

### Implementation for User Story 4

- [ ] T027 [US4] Add project document listing and download actions in ContosoDashboard/Pages/ProjectDetails.razor and related project service calls
- [ ] T028 [US4] Add task document attachment and upload flows in ContosoDashboard/Pages/Tasks.razor or task detail components
- [ ] T029 [US4] Enforce task/project association rules so uploaded task documents are tied to the task’s project in ContosoDashboard/Services/DocumentService.cs
- [ ] T030 [US4] Send project-member notifications when new project documents are added in ContosoDashboard/Services/NotificationService.cs and related document actions

**Checkpoint**: The project and task workflows should both work without depending on a full document admin surface.

---

## Phase 7: User Story 5 - Monitor Document Activity (Priority: P3)

**Goal**: Record document actions for audit and compliance and provide administrator-only reporting.

**Independent Test**: An administrator can review document activity and generate the required reports, while non-administrators cannot access those reports.

### Implementation for User Story 5

- [ ] T031 [US5] Add activity capture for upload, download, share, delete, and preview actions in ContosoDashboard/Services/DocumentActivityService.cs
- [ ] T032 [US5] Add admin-only reporting queries and summary generation in ContosoDashboard/Services/DocumentReportService.cs
- [ ] T033 [US5] Add administrator report UI and access control in ContosoDashboard/Pages/AdminDocuments.razor or equivalent protected page
- [ ] T034 [US5] Ensure non-admins are denied report access and protected document context is never leaked in report results

**Checkpoint**: Audit and reporting are in place and isolated to authorized administrators.

---

## Phase 8: User Story 6 - See Document Activity on the Dashboard (Priority: P2)

**Goal**: Provide a quick document summary and recent-document widget on the dashboard for current-user awareness.

**Independent Test**: A user with uploaded documents sees the five most recent uploads and a document count; a user with no uploads sees a clear empty state.

### Implementation for User Story 6

- [ ] T035 [US6] Extend the dashboard summary model and service data in ContosoDashboard/Services/DashboardService.cs and related dashboard models
- [ ] T036 [US6] Add the Recent Documents widget and empty-state handling in ContosoDashboard/Pages/Index.razor
- [ ] T037 [US6] Ensure dashboard summary values are scoped to the current authenticated user and available without exposing inaccessible or admin-only document data

**Checkpoint**: The dashboard is updated and independently functional for document activity visibility.

---

## Phase 9: Polish & Cross-Cutting Concerns

**Purpose**: Final quality, consistency, and validation across the whole document feature.

- [ ] T038 [P] Validate all document views for consistent empty-state, error, and success messaging across ContosoDashboard/Pages/ and related services
- [ ] T039 [P] Review permission logic across document create, read, update, delete, share, and report paths for IDOR and access-boundary correctness
- [ ] T040 [P] Run the quickstart validation and fix any edge-case regressions in ContosoDashboard/Pages/, ContosoDashboard/Services/, and ContosoDashboard/Data/
- [ ] T041 Review security headers, storage paths, and non-public file handling for training-environment compliance in ContosoDashboard/Program.cs and the storage service layer
- [ ] T042 Final build and smoke-test pass for the document feature with the existing Blazor app flow

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion; blocks all user stories
- **User Stories (Phase 3-8)**: Each depends on Foundational completion, and each story is independently testable
- **Polish (Phase 9)**: Depends on all desired stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Phase 2; no mandatory dependency on other stories
- **User Story 2 (P1)**: Can start after Phase 2; may build on US1 objects and queries but remains independently testable
- **User Story 3 (P1)**: Can start after Phase 2; depends on the core document model and auth helpers
- **User Story 4 (P2)**: Depends on the document model and project/task context, but should remain independent of the admin reporting flow
- **User Story 5 (P3)**: Depends on document actions and the permission model; should remain separate from dashboard UX
- **User Story 6 (P2)**: Depends on the user document record model and dashboard summary pattern

### Parallel Opportunities

- Setup tasks T002 and T003 can run in parallel after the feature structure is in place.
- Foundational tasks T005, T006, and T007 can be built in parallel if ownership is split across the model, data access, and storage layers.
- User Story 1 tasks T011-T015 can run in parallel as separate implementation streams once the foundational layer is complete.
- User Story 2 tasks T016-T020 can proceed in parallel with US1 final validation if the team is staffed for concurrent work.
- User Story 3 tasks T021-T026 can be developed independently of US5 and US6 when the service layer is stable.
- Final polish tasks T038-T042 can be run in parallel for review, smoke testing, and cleanup.

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Setup and Foundational tasks.
2. Complete User Story 1.
3. Validate the upload and personal-document journey independently.
4. Stop and confirm the happy path before continuing with additional stories.

### Incremental Delivery

1. Complete Setup + Foundational.
2. Deliver User Story 1 as the MVP document upload flow.
3. Add User Story 2 for browsing and access enforcement.
4. Add User Story 3 for management and sharing.
5. Add User Story 4 for project/task workflow integration.
6. Add User Story 5 and User Story 6 for audit/reporting and dashboard visibility.
7. Finish with cross-cutting polish and validation.

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together.
2. Split story work by priority:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
   - Developer D: User Story 4/6
   - Developer E: User Story 5
3. Use the final polish phase for integration validation across stories.

---

## Notes

- [P] tasks are parallelizable because they touch different files or different workflow streams.
- Each story task includes the required [USx] tag for traceability.
- The tasks target the existing application structure rather than introducing a new project boundary.
- All task descriptions include explicit file paths so the implementation can proceed without additional discovery.
