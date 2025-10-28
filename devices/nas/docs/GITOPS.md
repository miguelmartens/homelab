# GitOps Deployment with Portainer

This guide shows you how to deploy and automatically update stacks from this repository using Portainer's GitOps feature.

## 🎯 What is GitOps?

GitOps allows Portainer to automatically pull and deploy updates from a Git repository. When you update a `docker-compose.yml` file in this repository, Portainer will automatically detect and apply the changes.

## 📋 Prerequisites

1. Portainer installed and running (see [Portainer Installation](https://mariushosting.com/how-to-install-portainer-on-your-ugreen-nas/))
2. Access to Portainer web UI
3. This homelab repository available in a Git service (GitHub, GitLab, etc.)
4. Created shared folders in UGOS (see [Deployment Guide](DEPLOYMENT.md#important-create-shared-folders-before-deployment))

## 🚀 Setting Up a Stack with GitOps

### Step 1: Create Required Folders in UGOS

Before deploying, create the necessary folders in UGOS File Manager:

**For Duplicati:**
```
/volume1/data/duplicati/config/
/volume1/data/duplicati/backups/
```

**For Tailscale:**
```
/volume1/data/tailscale/state/
```

See [Deployment Guide](DEPLOYMENT.md#important-create-shared-folders-before-deployment) for detailed instructions.

### Step 2: Add Stack in Portainer

1. **Open Portainer**
   - Navigate to your NAS IP at `http://your-nas-ip:9000`
   - Login to Portainer

2. **Go to Stacks**
   - Click on "Stacks" in the left sidebar
   - Click "Add stack"

3. **Name Your Stack**
   - Enter a name, e.g., `duplicati` or `tailscale`

4. **Choose Git Repository**
   - Select "Git Repository" as the build method
   
5. **Configure Repository Settings**
   
   **Repository URL:**
   ```
   https://github.com/miguelmartens/homelab.git
   ```
   
   **Compose Path:** (Manually copy the path to your docker-compose.yml)
   
   For example:
   - **Duplicati:** `devices/nas/docker-compose/backups/duplicati/docker-compose.yml`
   - **Tailscale:** `devices/nas/docker-compose/infrastructure/tailscale/docker-compose.yml`

   📝 **How to find the path:**
   1. Look at the file structure in this repository
   2. From the root, follow the path to `docker-compose.yml`
   3. Copy the full path including `devices/nas/docker-compose/...`

6. **Enable GitOps Auto-Update**
   - ✅ Check "Enable GitOps"
   - ✅ Check "Auto-update from this Git repository"
   - Select "Action on update": `Recreate the stack`

7. **Configure Environment Variables**

   Click "Advanced mode" to expand environment variables section.

   **For Duplicati** (reference: `devices/nas/docker-compose/backups/duplicati/.env.example`):
   ```env
   PUID=1000
   PGID=1000
   TZ=Europe/Amsterdam
   DUPLICATI__WEBSERVICE_USERNAME=admin
   DUPLICATI__WEBSERVICE_PASSWORD=your_strong_password_here
   CONFIG_DIR=/volume1/data
   BACKUP_DIR=/volume1/data
   BACKUP_SOURCE=/volume1
   ```

   **For Tailscale** (reference: `devices/nas/docker-compose/infrastructure/tailscale/.env.example`):
   ```env
   TS_AUTHKEY=tskey-auth-xxxxxxxxxxxxxxxxxxxxxxxxxxx
   TS_STATE_DIR=/volume1/data
   TS_ROUTES=192.168.1.0/24
   TS_ACCEPT_DNS=true
   ```

   ⚠️ **Important:** Get actual values for sensitive variables:
   - Get Tailscale auth key from: https://login.tailscale.com/admin/settings/keys
   - Set strong passwords for Duplicati

7.5. **Create Shared Network (If Needed)**

Some stacks require a shared `lan` network. To create it:

1. Go to **Networks** in Portainer sidebar
2. Click **Add network**
3. Name: `lan`
4. Driver: `bridge`
5. Click **Create**

This network allows containers to communicate with each other.

8. **Deploy the Stack**
   - Click "Deploy the stack"
   - Portainer will clone the repository and deploy your stack

## 🔄 Automatic Updates

### How It Works

1. **Renovate bot** (if configured) creates PRs with updated Docker image versions
2. You merge the PR in GitHub
3. Portainer detects the change in the Git repository
4. Portainer automatically:
   - Pulls the latest changes
   - Recreates the stack with new image versions
   - Maintains your configuration and data

### Manual Trigger Update

If you want to manually trigger an update:

1. Go to **Stacks** → Select your stack
2. Click **Editor**
3. Click **Update & redeploy**

Or:

1. Go to **Stacks** → Select your stack
2. Click **GitOps**
3. Click **Force update**

## 📁 Example Stack Paths

| Service | Path |
|---------|------|
| Duplicati | `devices/nas/docker-compose/backups/duplicati/docker-compose.yml` |
| Tailscale | `devices/nas/docker-compose/infrastructure/tailscale/docker-compose.yml` |

## 🔐 Security Best Practices

### Environment Variables

1. **Never commit** actual values to Git
2. Use `.env.example` files as templates
3. Enter actual values in Portainer's environment section
4. Rotate passwords regularly
5. Store sensitive data securely

### Repository Access

- This repository is public (sanitized configs only)
- Your actual credentials stay in Portainer
- GitOps pulls configs, not secrets

## 🛠️ Troubleshooting

### Stack Fails to Deploy

1. **Check folder paths exist** in UGOS
2. **Verify environment variables** are set correctly
3. **Check permissions** on shared folders
4. **Review logs** in Portainer

### GitOps Not Updating

1. **Verify GitOps is enabled** in stack settings
2. **Check stack path** is correct
3. **Confirm repository URL** is accessible
4. **Force update** manually to test

### Permission Issues

```bash
# SSH into your NAS
ssh user@nas-ip

# Fix ownership
sudo chown -R 1000:1000 /volume1/data/duplicati
sudo chown -R 1000:1000 /volume1/data/tailscale
```

## 📚 Additional Resources

- [Portainer GitOps Documentation](https://docs.portainer.io/v/ce-2.19/user/docker/stacks/git)
- [NAS Deployment Guide](DEPLOYMENT.md)
- [Portainer Installation Guide](https://mariushosting.com/how-to-install-portainer-on-your-ugreen-nas/)

## 🔄 Workflow Summary

```
1. Clone this repository (get docker-compose.yml paths)
2. Create folders in UGOS
3. Set up Stack in Portainer with:
   - Repository URL
   - Compose Path
   - GitOps enabled
   - Environment variables
4. Deploy
5. Updates happen automatically when repository is updated
```

## 💡 Tips

- **Test locally** before deploying to production
- **Backup data** before major updates
- **Monitor logs** after deployment
- **Document** any custom changes you make
- **Use descriptive stack names** for easy management

