# Best Practices for NAS Homelab Configurations

## 🎯 Sanitization Guidelines

### DO ✅

- **Use Environment Variables**: All sensitive data should be in `.env` files
- **Use Placeholders**: Replace real values with obvious placeholders
- **Generic Paths**: Use `/data`, `/config` instead of user-specific paths
- **Generic Domains**: Use `.lan` or `.local` TLD instead of real domains
- **Placeholder IPs**: Use `192.168.1.0/24` or similar non-specific ranges
- **Document Requirements**: Explain what each placeholder should be replaced with

### DON'T ❌

- **No Real Secrets**: Never commit passwords, API keys, or tokens
- **No Personal Data**: Don't include hostnames, usernames, or personal info
- **No Real IPs**: Use placeholders for internal/external IPs
- **No Specific Paths**: Avoid `/Users/john/projects` style paths
- **No Hardcoded Ports**: Use environment variables for port mappings

## 📝 Docker Compose Guidelines

### Environment Variables

```yaml
# GOOD ✅
environment:
  - PASSWORD=${DB_PASSWORD}
  - API_KEY=${API_KEY}

# BAD ❌
environment:
  - PASSWORD=mySecretPassword123
  - API_KEY=sk_live_1234567890abcdef
```

### Volume Mounts

```yaml
# GOOD ✅
volumes:
  - ${DATA_DIR}/app:/data
  - ${CONFIG_DIR}/app:/config

# BAD ❌
volumes:
  - /home/john/myapp/data:/data
  - /opt/homelab/app/config:/config
```

### Network Configuration

```yaml
# GOOD ✅
networks:
  lan:
    external: true
    # Or use environment variable for subnet

# BAD ❌
networks:
  lan:
    driver: bridge
    ipam:
      config:
        - subnet: 192.168.1.0/24
          gateway: 192.168.1.1
```

## 🔒 Security Checklist

Before committing any configuration:

- [ ] No hardcoded credentials or secrets
- [ ] All sensitive data moved to environment variables
- [ ] `.env.example` created with placeholder values
- [ ] `.env` is in `.gitignore`
- [ ] IP addresses are placeholders
- [ ] File paths are generic
- [ ] Domains are placeholder (.lan, .local)
- [ ] Personal information removed
- [ ] Tested that secrets aren't in the compose file

## 📚 Documentation Standards

Every stack should include:

### 1. README.md

```markdown
# Stack Name

## Purpose
What does this stack do?

## Services
- Service 1: Brief description
- Service 2: Brief description

## Requirements
- Hardware requirements
- Software requirements
- Prerequisites

## Configuration

### Environment Variables

- `VAR_NAME`: Description of what this is for

## Deployment

Step-by-step instructions

## Maintenance

How to update, backup, etc.
```

### 2. Environment File Template

```env
# service-name/.env.example
# Required environment variables for this stack

# Network
LAN_NETWORK=192.168.1.0/24
TZ=America/Los_Angeles

# Service-specific variables
# VARIABLE_NAME=description of required value
API_KEY=your_api_key_here
DB_PASSWORD=change_this_in_production
```

## 🏗️ Structure Standards

### Directory Organization

```
docker-compose/
├── monitoring/
│   ├── stack.yml
│   ├── .env.example
│   └── README.md
├── media/
│   ├── stack.yml
│   ├── .env.example
│   └── README.md
```

### File Naming

- **Compose files**: `stack.yml` or `docker-compose.yml`
- **Environment**: `.env.example` (committed), `.env` (gitignored)
- **Documentation**: `README.md` (required in each stack directory)

## 🚀 Deployment Best Practices

### Portainer Deployment

1. **Stack Name**: Use descriptive names (e.g., "monitoring", "media-server")
2. **Editor Mode**: Use editor for complex configurations
3. **Environment**: Set environment variables in the UI
4. **Labels**: Use labels for organization
5. **Healthchecks**: Enable health checks for critical services

### Version Control

- Keep compose files in version control
- Never commit `.env` files
- Use semantic versioning for tags
- Document breaking changes
- Tag stable configurations

### Updates

1. Pull updated compose file
2. Review changes (especially new environment variables)
3. Update `.env` file if needed
4. Deploy to staging first
5. Backup before production deployment
6. Monitor logs after deployment

## 🧪 Testing

### Validation

```bash
# Validate compose file syntax
docker-compose config

# Dry run (check what would happen)
docker-compose config --services

# Check for secrets (grep patterns)
grep -r "password\|secret\|key\|token" --include="*.yml" ./
```

### Pre-commit Checks

Before committing:
1. Validate syntax: `docker-compose config`
2. Search for secrets: `grep -i "password\|secret\|api_key" *.yml`
3. Verify `.env.example` exists
4. Check documentation is complete
5. Test locally if possible

## 📊 Monitoring

### Essential Metrics

- Container health status
- Resource usage (CPU, RAM, disk)
- Log analysis
- Network connectivity
- Storage usage

### Tools

- Use healthchecks in compose files
- Monitor through Portainer
- Set up external monitoring (Prometheus, Grafana)
- Alert on failures

## 🔄 Maintenance

### Regular Tasks

- **Weekly**: Check container updates
- **Monthly**: Review and update configurations
- **Quarterly**: Review and rotate secrets
- **Ongoing**: Monitor logs and metrics

### Backup Strategy

```bash
# Backup script example
#!/bin/bash
BACKUP_DIR=/backup
DATE=$(date +%Y%m%d)

# Backup volumes
docker run --rm \
  -v volume_name:/data \
  -v $BACKUP_DIR:/backup \
  alpine tar czf /backup/volume-$DATE.tar.gz /data

# Export compose configuration
cp docker-compose.yml $BACKUP_DIR/compose-$DATE.yml
cp .env $BACKUP_DIR/env-$DATE  # Sanitized version
```

## 📖 Documentation Tips

- Be specific about requirements
- Include example configurations
- Document special considerations
- Provide troubleshooting sections
- Keep documentation up to date
- Link to official documentation
- Include version information

## 🎓 Learning Resources

- [Docker Compose Best Practices](https://docs.docker.com/compose/production/)
- [Portainer Documentation](https://docs.portainer.io)
- [12 Factor App](https://12factor.net/)
- [Container Best Practices](https://docs.docker.com/develop/dev-best-practices/)

