# Python Dev Container

Development environment for Python projects. Container uses `mcr.microsoft.com/vscode/devcontainers/python:latest`, installs `requirements.txt` and `requirements-dev.txt`, and runs as `vscode`.

## Use

Install Docker and Visual Studio Code with [Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers) support.

1. Clone repository.
2. Open repository in VS Code.
3. Run **Dev Containers: Reopen in Container**.

GitHub Codespaces also detects `.devcontainer/devcontainer.json` automatically.

## Validation

GitHub Actions validates Dockerfile changes pushed to `dev` and pull requests targeting `main`.
