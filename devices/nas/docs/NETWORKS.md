# Network Configuration

This guide explains network configuration for Docker stacks in Portainer.

## 🏗️ Network Options

### 1. Shared LAN Network (Recommended)

Most stacks use an external `lan` network to communicate with each other.

**Advantages:**
- Containers can communicate by name
- Isolation from other projects
- Better organization
- Easy to add/remove containers

**How to Create:**

1. Go to **Networks** in Portainer
2. Click **Add network**
3. Configure:
   - **Name:** `lan`
   - **Driver:** `bridge`
4. Click **Create**

All your stacks will use this shared network.

### 2. Isolated Network

If you remove the `networks` section from docker-compose.yml:
- Container has no network configuration
- Docker Compose creates isolated network automatically
- Name: `[project]_default` (e.g., `duplicati_default`)

**When to use:**
- Single standalone service
- Don't need to communicate with other containers
- Extra security isolation

### 3. Host Network Mode

Uses the host's network directly.

**When to use:**
- VPN services (like Tailscale)
- Need direct access to host network interfaces
- Special network requirements

**Example:**
```yaml
network_mode: host
```

## 📋 Network Configuration Examples

### External LAN Network (Most Common)

```yaml
services:
  my-service:
    image: myimage:latest
    networks:
      - lan

networks:
  lan:
    external: true
```

### Isolated Network

```yaml
services:
  my-service:
    image: myimage:latest
    # No networks section = isolated
```

### Host Network Mode

```yaml
services:
  my-service:
    image: myimage:latest
    network_mode: host
```

## 🔍 Finding Network Information

### Check Current Networks

**In Portainer:**
- Go to **Networks**
- See all created networks
- View connected containers

**Via CLI:**
```bash
docker network ls
docker network inspect lan
```

### Check Container Network

```bash
docker inspect <container-name> | grep NetworkMode
```

## 🔧 Common Network Issues

### "Network not found" Error

**Problem:** `lan` network doesn't exist

**Solution:**
1. Create the `lan` network in Portainer
2. Or remove `networks` section to use isolated network

### Containers Can't Communicate

**Problem:** Containers on different networks

**Solution:**
- Use the same network for all related containers
- Or connect container to additional network:
  ```bash
  docker network connect lan container-name
  ```

### Port Already in Use

**Problem:** Port conflict

**Solution:**
- Check what's using the port: `netstat -tulpn | grep :8200`
- Change the port mapping in compose file
- Or remove conflicting service

## 📚 Recommended Network Setup

For most homelab setups:

```yaml
# Shared network for all services
networks:
  lan:
    external: true
```

**Benefits:**
- Easy container-to-container communication
- DNS-based service discovery
- Better organization
- Centralized network management

## 🚨 Security Considerations

### Isolated Networks
- Maximum isolation
- No accidental cross-service communication
- More complex if services need to communicate

### Shared Networks
- Convenient for service communication
- Proper security still enforced by application
- Easier management

### Host Network
- Direct host access
- No Docker network isolation
- Only for special use cases (VPN, etc.)

## 💡 Best Practices

1. **Use `lan` network** for most services
2. **Isolated networks** for standalone services
3. **Host network** only when necessary
4. **Document** any special network requirements
5. **Test** network connectivity after deployment

## 🛠️ Troubleshooting

### View Container Networks
```bash
# List all networks
docker network ls

# Inspect specific network
docker network inspect lan

# See what containers are connected
docker network inspect lan | grep Containers
```

### Connect Container to Network
```bash
# Connect existing container
docker network connect lan container-name

# Or recreate with new network
docker-compose down
# Edit compose file
docker-compose up -d
```

### Clean Up Unused Networks
```bash
# Remove unused networks
docker network prune
```

