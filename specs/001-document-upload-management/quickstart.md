# Quickstart: Document Upload and Management Validation

## Prerequisites

- .NET 9 SDK installed
- The ContosoDashboard app running locally
- A browser session with one of the seeded demo users

## Run the app

```bash
cd /home/lkhalil_silamir/TrainingProjects/ContosoDashboard/ContosoDashboard
dotnet run
```

## Validation scenarios

### 1. Upload success and metadata capture
1. Sign in as an employee or project manager.
2. Open the document upload flow.
3. Select a supported file under 25 MB.
4. Enter a title and category.
5. Submit the upload.
6. Confirm the upload shows progress and completes successfully.
7. Confirm the document appears in the personal document list with title, category, upload date, file size, and project association when applicable.

### 2. Rejected uploads
1. Attempt an upload with a file larger than 25 MB.
2. Attempt a file with an unsupported extension or a file that fails the security validation.
3. Confirm each file is rejected with a clear reason and no incomplete document is shown as available.

### 3. Permission enforcement
1. Sign in as one user and upload a document for a project they are assigned to.
2. Sign in as another user without access to that project or document.
3. Confirm the second user cannot view, preview, download, edit, replace, delete, or share the protected document.

### 4. Search, filter, and sort
1. Upload several documents across categories and projects.
2. Use the document list to sort by title, date, category, and size.
3. Apply category and project filters and search by title, tags, uploader, or project name.
4. Confirm only accessible matches return and the empty state appears when nothing matches.

### 5. Dashboard visibility
1. Upload one or more documents as a user.
2. Open the dashboard.
3. Confirm the recent documents widget shows the latest five uploads and the summary card reflects the user’s document count.
4. Confirm the empty state is shown for a user with no uploaded documents.

### 6. Share and audit trail
1. Share a document with another user or team.
2. Confirm the recipient sees it in Shared with Me and receives an in-app notification.
3. Confirm the audit log records the share action with actor and document context.

## Expected outcomes

- Valid uploads succeed without exposing incomplete storage states.
- Invalid uploads fail clearly without becoming visible to users.
- Access control remains enforced across list, search, preview, download, and management actions.
- Dashboard data and document records remain consistent with the current user’s permissions.
