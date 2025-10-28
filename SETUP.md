# Setup Guide

Quick setup guide for this homelab repository.

## 🔧 Initial Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/homelab.git
cd homelab
```

### 2. Enable GitHub Automation

#### Install Renovate Bot

1. Go to [Renovate GitHub App](https://github.com/apps/renovate)
2. Click "Configure" 
3. Select your repository
4. Grant necessary permissions
5. Click "Install"

Renovate will automatically:
- Create PRs for outdated Docker images
- Group related updates
- Auto-merge patch updates (with approval)
- Require manual review for major updates

#### Verify Security Scanning

Gitleaks security scanning is already configured via GitHub Actions:
- Runs on every push and pull request
- Daily scheduled scans at midnight UTC
- Automatically blocks commits with detected secrets

No additional setup required.

## 📦 Using the Configurations

### For UGOS (Ugreen NAS)

1. **Install Portainer** (if not already installed)
   - Follow the [Portainer Installation Guide](https://mariushosting.com/how-to-install-portainer-on-your-ugreen-nas/)

2. **Create Required Folders**
   - Before deploying any stack, create the necessary folders in UGOS File Manager
   - See [NAS Deployment Guide](devices/nas/docs/DEPLOYMENT.md#important-create-shared-folders-before-deployment)

3. **Deploy a Stack**

   **Option A: GitOps Deployment (Recommended)**
   - Use Portainer's GitOps feature for automatic updates
   - See [GitOps Deployment Guide](devices/nas/docs/GITOPS.md) for step-by-step instructions

   **Option B: Manual Deployment**
   - Copy a docker-compose file from `devices/nas/docker-compose/`
   - Create a `.env` file from the provided `.env.example`
   - Update environment variables with your values
   - Deploy via Portainer

See [NAS README](devices/nas/README.md) for detailed instructions.

## 🔄 Maintenance

### Automatic Updates

Renovate will automatically:
- Create PRs when Docker images have updates
- Update GitHub Actions workflows
- Run weekly on Sundays

Review and merge PRs as they appear to keep configurations updated.

### Manual Updates

To manually update images:

1. **Via Portainer:**
   - Open Portainer
   - Go to Stacks
   - Click on your stack
   - Click "Editor"
   - Update image tags
   - Update the stack

2. **Via Docker Compose CLI:**
   ```bash
   docker-compose pull
   docker-compose up -d
   ```

## 🔒 Security

### Secret Management

- **Never commit actual secrets** - Use `.env` files (gitignored)
- **Use placeholder values** in committed files
- **Validate before commit** - Gitleaks will catch secrets

### Best Practices

1. Keep `.env` files local and gitignored
2. Use strong, unique passwords
3. Rotate credentials periodically
4. Review security scan results
5. Test updates in staging first

## 📚 Resources

- [Renovate Documentation](https://docs.renovatebot.com/)
- [Gitleaks Documentation](https://github.com/gitleaks/gitleaks)
- [Portainer Documentation](https://docs.portainer.io/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## 🆘 Troubleshooting

### Renovate Not Creating PRs

- Ensure Renovate app is installed and enabled
- Check [Renovate Dashboard](https://github.com/apps/renovate) for activity
- Verify `renovate.json` configuration is valid

### Gitleaks Failing

- Review the error message in GitHub Actions
- Ensure no actual secrets in committed files
- Check `.env.example` files use placeholders only

### Container Failures

- Verify folders exist in UGOS before deployment
- Check environment variables are set correctly
- Review container logs in Portainer

