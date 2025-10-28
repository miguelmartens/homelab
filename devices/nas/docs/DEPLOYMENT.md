# Deployment Guide for NAS

## Prerequisites

- NAS device (e.g., Ugreen NAS DXP2800) with UGOS installed
- Portainer running and accessible
- Docker installed and configured
- SSH or web interface access to the device

## Initial Setup

### 1. Access Portainer

1. Open the UGOS web interface
2. Navigate to Portainer (usually accessible at `http://nas-ip:9000`)
3. Login with your credentials

### 2. Configure Environment Variables

Create a `.env` file in your chosen docker-compose directory:

```bash
# SSH into your NAS or use Portainer terminal
cd /path/to/your/compose/files
cp .env.example .env
nano .env  # or use Portainer's file editor
```

Fill in the actual values:
- Network IP ranges
- Timezone
- Directories
- Application-specific settings

## Deploying a Stack

### Method 1: Via Portainer Web UI

1. Login to Portainer
2. Go to **Stacks** → **Add stack**
3. Name your stack (e.g., "monitoring")
4. **Editor mode**: Paste or upload your `docker-compose.yml`
5. Configure environment variables inline
6. Click **Deploy the stack**

### Method 2: Via Portainer CLI

If you have SSH access:

```bash
# SSH into your NAS
ssh user@nas-ip

# Navigate to compose directory
cd /path/to/compose/files

# Deploy via docker-compose
docker-compose up -d
```

### Method 3: Git-Sync (Advanced)

If you want to keep configurations in sync with this repo:

1. Clone this repository to your NAS
2. Use Portainer's Git sync feature
3. Update configurations as needed

## Managing Stacks

### View Logs

- **Portainer UI**: Stacks → Your Stack → Container logs
- **CLI**: `docker-compose logs -f <service-name>`

### Update Containers

- **Portainer UI**: Stacks → Your Stack → Editor → Update & redeploy
- **CLI**: `docker-compose pull && docker-compose up -d`

### Remove Stacks

- **Portainer UI**: Stacks → Your Stack → Remove
- **CLI**: `docker-compose down`

## Backup and Restore

### Backup

```bash
# Backup volumes
docker run --rm -v <volume-name>:/data -v $(pwd):/backup \
  alpine tar czf /backup/backup.tar.gz /data

# Export volumes
docker run --rm -v <volume-name>:/data -v $(pwd):/backup \
  alpine sh -c "cd /data && tar czf /backup/data.tar.gz ."
```

### Restore

```bash
# Restore volumes
docker run --rm -v <volume-name>:/data -v $(pwd):/backup \
  alpine tar xzf /backup/backup.tar.gz -C /data
```

## Troubleshooting

### Container won't start

1. Check logs: `docker-compose logs <service-name>`
2. Verify environment variables
3. Check port conflicts
4. Verify volume permissions

### Permission issues

```bash
# Fix ownership
sudo chown -R 1000:1000 /path/to/data

# Or set appropriate permissions
chmod -R 755 /path/to/data
```

### Network issues

1. Verify network configuration in compose file
2. Check firewall rules
3. Ensure services are on the same network

### Disk space

```bash
# Check disk usage
df -h

# Clean up Docker
docker system prune -a
docker volume prune
```

## Monitoring

Keep an eye on:
- Container health
- Resource usage (CPU, RAM, disk)
- Logs for errors
- Network connectivity

## Best Practices

1. ✅ Always use `.env` files for configuration
2. ✅ Regularly backup volumes
3. ✅ Keep containers updated
4. ✅ Monitor resource usage
5. ✅ Test updates in staging first
6. ✅ Document any custom configurations

## UGOS Specific Considerations

- Storage locations may be specific to UGOS paths
- Some hardware acceleration features may be available
- UI access through UGOS portal
- Container access to NAS storage volumes

## Support

- UGOS Documentation
- Portainer Community
- Docker Compose Documentation
- Container-specific documentation

