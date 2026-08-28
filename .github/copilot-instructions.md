# Copilot Cloud Agent Onboarding

## Repository snapshot
- This repository is currently very small and contains:
  - `/home/runner/work/blob1/blob1/README.md`
  - `/home/runner/work/blob1/blob1/.github/workflows/main_yiuiiuiuiiuui.yml`
- The workflow indicates a .NET Azure Functions deployment pipeline.

## Primary technology and CI/CD behavior
- GitHub Actions workflow: `main_yiuiiuiuiiuui.yml`
- Trigger: push to `main` and manual `workflow_dispatch`
- Runner: `windows-latest`
- .NET SDK: `6.0.x`
- Build command used in CI:
  - `dotnet build --configuration Release --output ./output`
- Deployment action: `Azure/functions-action@v1`

## Practical guidance for first-time agents
- Start by reading the workflow file to understand build/deploy expectations before changing code or pipeline behavior.
- Keep edits minimal and focused; this repo currently has very little structure and no visible test suite.
- If you modify workflow steps, preserve required Azure deployment inputs (`app-name`, `slot-name`, `package`, and publish profile secret reference).
- Do not hardcode secrets; continue using GitHub Actions secrets.

## Validation guidance
- There is no explicit test command documented in the repository at this time.
- For .NET-related changes, use the existing CI-aligned build command:
  - `dotnet build --configuration Release --output ./output`
- For workflow-only or documentation-only edits, verify formatting and file paths carefully.

## Errors encountered during onboarding
- No repository/tooling errors were encountered while preparing this onboarding file.
- No workaround steps were needed.
