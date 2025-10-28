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

