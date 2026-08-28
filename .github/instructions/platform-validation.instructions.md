# Platform Validation Instructions

This document describes the platform validation guidelines for this repository.

## Build Validation

- Use .NET SDK `6.0.x` for all builds.
- Build command: `dotnet build --configuration Release --output ./output`
- Runner: `windows-latest`

## Deployment Validation

- Deployment target: Azure Functions
- Deployment action: `Azure/functions-action@v1`
- Required inputs: `app-name`, `slot-name`, `package`, and publish profile secret reference.
- Do not hardcode secrets; use GitHub Actions secrets.

## CI/CD Pipeline

- Workflow file: `.github/workflows/main_yiuiiuiuiiuui.yml`
- Triggers: push to `main` and manual `workflow_dispatch`

## General Guidelines

- Keep edits minimal and focused.
- Preserve required Azure deployment inputs when modifying workflow steps.
- Verify formatting and file paths carefully for workflow-only or documentation-only edits.
