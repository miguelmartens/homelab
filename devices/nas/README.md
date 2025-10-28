# Ugreen NAS DXP2800 (UGOS)

Docker compose configurations for running containers on the Ugreen NAS DXP2800 with UGOS, designed to be deployed via Portainer.

## 📋 Overview

The Ugreen NAS DXP2800 runs UGOS (Ugreen Operating System) and supports Docker containers via Portainer. This directory contains sanitized docker-compose configurations that can be safely used in a public repository.

## 🎯 Prerequisites

Portainer is installed and running on the NAS following the [Portainer Installation Guide for Ugreen NAS](https://mariushosting.com/how-to-install-portainer-on-your-ugreen-nas/).

## 📁 Directory Structure

```
nas/
├── docker-compose/    # Docker compose files for various services
│   ├── monitoring/    # Monitoring stacks (Grafana, Prometheus, etc.)
│   ├── media/         # Media stacks (Plex, Jellyfin, etc.)
│   ├── development/   # Development tools
│   └── infrastructure/# Infrastructure services
└── docs/              # Documentation specific to UGOS deployment
```

## 🚀 Deployment

### ⚠️ Prerequisites

**IMPORTANT:** Before deploying any stack, create the necessary folders in UGOS Shared Folders.

1. Open UGOS File Manager
2. Navigate to your data volume (usually `Volume1` or `Volume2`)
3. Create folders matching the volume paths in your compose files
4. Set appropriate permissions (Read/Write for Docker user)
5. Update environment variables in `.env` with correct paths

Example structure:
```
/volume1/data/
├── tailscale/state/
├── duplicati/config/
└── duplicati/backups/
```

See [Deployment Guide](docs/DEPLOYMENT.md#important-create-shared-folders-before-deployment) for detailed instructions.

### Via Portainer

1. Navigate to Stacks in Portainer
2. Click "Add stack"
3. Choose your compose file from `docker-compose/` subdirectories
4. Update the environment variables with your specific values
5. Deploy the stack

### Environment Configuration

Each docker-compose file should have a corresponding `.env.example` file showing required environment variables. Copy this to `.env` locally:

```bash
cp .env.example .env
# Edit .env with your actual values (this file is gitignored)
```

## 📝 Configuration Guidelines

When adding new configurations:

1. **Network Configuration**
   - Use placeholder IP ranges (e.g., `192.168.1.0/24` for LAN)
   - Replace internal domains with `.lan` or `.local` TLD
   - Use environment variables for IPs and domains

2. **Volume Mounts**
   - Use generic paths: `/data/app-name`, `/config/app-name`
   - Document required permissions
   - Avoid absolute user-specific paths

3. **Ports**
   - Use environment variables for port mappings
   - Avoid hardcoded ports that might conflict

4. **Secrets**
   - Never commit actual secrets
   - Use `.env.example` to show structure
   - Consider Docker secrets for sensitive data

## 🔧 UGOS Specific Notes

- UGOS runs on a Linux-based system
- Default Docker storage location may vary
- Some containers may need specific device access
- Check UGOS firmware version compatibility

## 📦 Available Stacks

Document your docker-compose stacks here as you add them:

### Example: Monitoring Stack
- **File:** `docker-compose/monitoring/stack.yml`
- **Services:** Grafana, Prometheus, Node Exporter
- **Description:** Complete monitoring solution for your homelab

## 🛠️ Maintenance

- Regularly update container images
- Review and update environment variables
- Keep documentation up to date
- Test deployments in a non-production environment first

## 📚 Resources

- [Portainer Installation Guide for Ugreen NAS](https://mariushosting.com/how-to-install-portainer-on-your-ugreen-nas/)
- [UGOS Documentation](https://www.ugreen.com/support)
- [Portainer Documentation](https://docs.portainer.io)
- [Docker Compose Best Practices](https://docs.docker.com/compose/production/)

