# Homelab

A public repository containing sanitized homelab configurations and templates for various hardware devices and services.

## 🔐 Sanitization Policy

All configurations in this repository are **sanitized** and safe for public consumption:
- ✅ No secrets or credentials
- ✅ No sensitive file paths
- ✅ No identifying network information (internal IPs use placeholders)
- ✅ No personal data

Secrets are managed externally (e.g., Docker secrets, environment files in `.gitignore`)

## 🔒 Security

This repository is automatically scanned for secrets and sensitive information on every push and pull request using [Gitleaks](https://github.com/gitleaks/gitleaks-action). Any accidental commits containing secrets will be flagged and the workflow will fail.

**Security checks:**
- ✅ Automated secret scanning on every commit
- ✅ Daily scheduled scans
- ✅ Pull request protection
- ✅ Placeholder values in all committed files

## 🔄 Automated Updates

This repository uses [Renovate](https://github.com/renovatebot/renovate) to automatically keep Docker images and dependencies up to date.

**Features:**
- 🔄 Automatic PRs for outdated Docker images
- 📦 Groups related updates together
- ✅ Auto-merges patch updates (when approved)
- ⚠️ Major updates require manual review
- 📅 Weekly update schedule (Sundays)

**Setup:** Install the [Renovate GitHub App](https://github.com/apps/renovate) on your repository.

## 📁 Repository Structure

```
├── devices/           # Device-specific configurations
│   └── nas/
│       ├── docker-compose/
│       └── docs/
└── services/         # Service-specific configurations
```

## 🖥️ Supported Devices

### Ugreen NAS DXP2800 (UGOS)

Docker compose configurations designed to run on Portainer.

**Directory:** `devices/nas/`

Configuration files are designed to be:
- Deployed via Portainer
- Environment-aware (development/production)
- Extensible and maintainable

## 🚀 Usage

### GitOps Deployment (Recommended)

Automatically deploy and update stacks using Portainer's GitOps feature:

1. In Portainer, add a stack from Git Repository
2. Repository URL: `https://github.com/miguelmartens/homelab.git`
3. Compose Path: `devices/nas/docker-compose/[category]/[service]/docker-compose.yml`
4. Enable GitOps for automatic updates
5. Configure environment variables
6. Deploy

See [GitOps Guide](devices/nas/docs/GITOPS.md) for detailed instructions.

### Manual Deployment

1. Navigate to the appropriate device directory
2. Copy the desired `docker-compose.yml` file
3. In Portainer, create a new stack
4. Update any placeholder values with your specific configuration
5. Deploy

### Local Development

```bash
# Example for NAS
cd devices/nas/docker-compose
docker-compose up -d
```

## 📝 Adding Configurations

When adding new configurations:
1. **Create required folders in UGOS File Manager first** - Volume paths must exist before deployment
2. Ensure all secrets are moved to environment files (`.env.example` provided)
3. Replace specific paths with generic ones
4. Use placeholder IPs and domains
5. Update this README if adding new devices/services
6. Document any special requirements

See [NAS Deployment Guide](devices/nas/docs/DEPLOYMENT.md#important-create-shared-folders-before-deployment) for folder creation instructions.

## 🤝 Contributing

This is a public repository showcasing homelab best practices. Feel free to use these configurations as templates for your own homelab.

## 📄 License

[MIT](LICENSE) - Free to use for your own homelab projects