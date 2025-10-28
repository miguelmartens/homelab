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

### Portainer Deployment

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
1. Ensure all secrets are moved to environment files (`.env.example` provided)
2. Replace specific paths with generic ones
3. Use placeholder IPs and domains
4. Update this README if adding new devices/services
5. Document any special requirements

## 🤝 Contributing

This is a public repository showcasing homelab best practices. Feel free to use these configurations as templates for your own homelab.

## 📄 License

[MIT](LICENSE) - Free to use for your own homelab projects