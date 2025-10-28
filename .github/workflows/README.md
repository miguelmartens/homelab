# GitHub Workflows

This directory contains GitHub Actions workflows for the homelab repository.

## Workflows

### Renovate

Automatic dependency updates for Docker images and GitHub Actions.

**Configuration:** `renovate.json` (at repository root)

**Features:**
- Automatically creates PRs for outdated Docker images
- Groups related updates together
- Auto-merges patch updates (with approval)
- Requires manual review for major version updates
- Runs weekly on Sundays

**What it updates:**
- Docker images in all docker-compose files
- GitHub Actions in workflow files

See [Renovate Dashboard](https://github.com/apps/renovate) for activity and settings.

### YAML Lint

Validates YAML syntax for all YAML files in the repository using the official [yamllint](https://yamllint.readthedocs.io/) tool.

**Configuration:** `.yamllint.yml` (at repository root)

**What it checks:**
- YAML syntax validity
- Indentation consistency (2 spaces)
- Line length (max 120)
- Proper YAML formatting
- Comments and empty lines

**Triggers:**
- Push to main branches
- Pull requests targeting main branches
- Weekly scheduled runs
- Manual trigger via workflow_dispatch

**Note:** yamllint automatically detects GitHub Actions and formats output accordingly.

### Gitleaks Security Scan

Automatically scans the repository for secrets and sensitive information that may have been accidentally committed.

**Triggers:**
- Push to main branches (main, develop, master)
- Pull requests targeting main branches
- Scheduled daily at midnight UTC
- Manual trigger via workflow_dispatch

**What it scans for:**
- API keys and tokens
- Passwords and credentials
- SSH keys
- Database connection strings
- Cloud provider credentials
- And more...

**Fail behavior:** 
The workflow will fail if any secrets are detected, preventing them from being merged.

**Important:** This is a public repository, so ALL secrets detected will be flagged. Make sure to:
- Never commit actual `.env` files (they're in `.gitignore`)
- Use placeholder values in committed files
- Store actual secrets securely outside the repository
- Review any workflow failures for accidentally committed secrets

