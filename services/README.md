# Services

Service-specific configurations that can be deployed across multiple devices or platforms.

## 📝 Purpose

This directory contains docker-compose configurations for specific services or applications that:
- Can be deployed on multiple device types
- Are platform-agnostic
- Serve a specific purpose (media server, monitoring, etc.)

## 🗂️ Organization

Organize services by their primary function:

### Potential Services

- **Media Servers**: Plex, Jellyfin, Kodi
- **File Management**: Nextcloud, FileBrowser
- **Development**: VS Code Server, GitLab
- **Monitoring**: Grafana, Prometheus, Netdata
- **Networking**: Cloudflare Tunnel, WireGuard
- **Automation**: Home Assistant, Node-RED
- **Backups**: Duplicati, BorgBackup

## 📋 Adding a Service

When adding a new service:

1. Create a directory with a descriptive name
2. Add the following files:
   - `docker-compose.yml` - Main configuration
   - `.env.example` - Environment variables template
   - `README.md` - Service documentation
3. Ensure all sensitive information is sanitized
4. Include deployment instructions
5. Add any special requirements or dependencies

## 🔒 Security

- No hardcoded credentials
- Use environment variables for all secrets
- Provide `.env.example` templates
- Document all required permissions

## 📚 Documentation

Each service should include:

- **Purpose**: What the service does
- **Requirements**: Hardware/software prerequisites
- **Configuration**: How to configure it
- **Deployment**: Step-by-step deployment guide
- **Maintenance**: How to update and maintain
- **Troubleshooting**: Common issues and solutions

## 🚀 Using Services

1. Browse the available services
2. Read the service-specific README
3. Copy the docker-compose.yml file
4. Create your `.env` file from the example
5. Deploy using Portainer, Docker CLI, or preferred method

## 🔄 Device Integration

Some services in this directory may be referenced by device-specific configurations in `devices/`. This allows for:
- Shared service definitions
- Consistency across devices
- Easier maintenance and updates

