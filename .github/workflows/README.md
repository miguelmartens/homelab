# GitHub Workflows

This directory contains GitHub Actions workflows for the homelab repository.

## Workflows

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

