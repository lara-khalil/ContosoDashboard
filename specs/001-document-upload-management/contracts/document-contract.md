# Document Contracts

## Overview

This project is a web app rather than a dedicated service API. The document feature exposes its behavior through Blazor page interactions and service methods, with a focus on permissioned actions and persisted metadata.

## Upload contract

### Action
Upload one or more supported documents with metadata.

### Inputs
- Title: required string
- Category: required predefined category
- Description: optional string
- ProjectId: optional integer
- Tags: optional string list or delimited text
- Files: one or more uploaded files

### Validation
- file count > 0
- each file <= 25 MB
- file type is supported
- security scan must pass before storage
- metadata and permissions must be validated before persistence

### Output
- success status per file
- stored metadata record
- audit activity entry
- user-visible progress and completion message

## Access contract

### List/search/download operations
- Return only documents accessible to the current user
- Exclude inaccessible document metadata from list and search results
- Deny direct previews and downloads when access is missing

### Shared access
- Recipients receive in-app notification when shared
- Shared documents appear in the recipient’s Shared with Me or equivalentfiltered list

## Management contract

### Edit / replace / delete
- Document owners may edit metadata and replace files when the file is valid
- Project managers may manage documents in their own projects
- Deletion removes both the metadata record and the stored file
- Audit records are created for each management action
