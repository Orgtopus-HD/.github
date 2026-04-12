# .github

Org-wide shared GitHub Actions reusable workflows for the Orgtopus platform.

## Reusable Workflows

| Workflow | Used By | Purpose |
|----------|---------|---------|
| `python-service-ci.yml` | Backend services, agents, orgtopus-shared | Lint (ruff/black) + test (pytest) with optional PostgreSQL |
| `docker-build-push.yml` | All repos with Docker images | Build and push to `ghcr.io/orgtopus-hd/` |
| `go-ci.yml` | operator | Go lint (golangci-lint) + test |
| `node-ci.yml` | frontend | TypeScript check + ESLint + Vitest + optional Playwright E2E |
| `helm-ci.yml` | helm-charts | Helm dependency update + lint + template validation |

## Usage

In your repo's `.github/workflows/ci.yml`:

```yaml
name: CI
on: [pull_request, push]
jobs:
  ci:
    uses: Orgtopus-HD/.github/.github/workflows/python-service-ci.yml@main
    with:
      coverage-threshold: 60
    secrets: inherit
  build:
    if: github.ref == 'refs/heads/main'
    uses: Orgtopus-HD/.github/.github/workflows/docker-build-push.yml@main
    with:
      image-name: my-service
      needs-github-token-build-arg: true
    secrets: inherit
```

## Updating CI

Changes to these workflows automatically apply to all repos on their next CI run. No need to update individual repos.
