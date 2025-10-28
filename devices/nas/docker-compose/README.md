# Docker Compose Stacks for NAS

This directory contains docker-compose configurations organized by category.

## 📂 Directory Structure

- **monitoring/** - Monitoring and observability tools (Grafana, Prometheus, etc.)
- **media/** - Media servers and related services (Plex, Jellyfin, etc.)
- **development/** - Development tools and IDEs
- **infrastructure/** - Core infrastructure services (DNS, reverse proxy, etc.)

## 📝 Usage

Each subdirectory should contain:
- `docker-compose.yml` or `stack.yml` - Main compose file
- `.env.example` - Example environment variables (required!)
- `README.md` - Documentation specific to that stack

## 🚀 Quick Start

1. Choose a stack from the subdirectories
2. Copy the compose file to Portainer or your deploy location
3. Create a `.env` file based on `.env.example`
4. Update the environment variables with your values
5. Deploy the stack

## ⚠️ Important Notes

- **NEVER commit actual `.env` files** with secrets
- Always use placeholders in compose files
- Test in a non-production environment first
- Keep backups of your configurations

## 📋 Required Files Template

When creating a new stack, include at minimum:

1. **docker-compose.yml** - Your compose configuration
2. **.env.example** - Required environment variables
3. **README.md** - Stack-specific documentation

Example `.env.example`:

```env
# Stack: Monitoring
# Description: Prometheus + Grafana monitoring stack

# Network
LAN_NETWORK=192.168.1.0/24

# Timezone
TZ=America/Los_Angeles

# Service-specific variables
GRAFANA_ADMIN_PASSWORD=change_me
```

Example minimal `README.md`:

```markdown
# Stack Name

Brief description of what this stack does.

## Services

- Service 1: Description
- Service 2: Description

## Requirements

List any requirements or prerequisites.

## Deployment

Brief deployment instructions.
```

## 🔒 Security Checklist

Before committing any compose file:

- [ ] No hardcoded passwords or API keys
- [ ] No real IP addresses (use placeholders)
- [ ] No personal or identifying information
- [ ] All secrets in `.env.example` with placeholder values
- [ ] `.env` is in `.gitignore`
- [ ] Paths are generic (not user-specific)

## 📦 Adding New Stacks

1. Create a new subdirectory in the appropriate category
2. Add `docker-compose.yml` with sanitized configuration
3. Add `.env.example` with required variables
4. Add a `README.md` with setup instructions
5. Update the main README if adding a new category

## 🧪 Testing

Before deploying:

1. Validate compose file: `docker-compose config`
2. Test locally if possible
3. Verify all placeholders are present
4. Check for exposed sensitive information
5. Test deployment in a safe environment

