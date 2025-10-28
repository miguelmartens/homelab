# Infrastructure Services

Core infrastructure services for network and system management.

## Available Stacks

### Tailscale

VPN service providing secure network access using WireGuard.

**File:** `tailscale.yml`

**Features:**
- Mesh VPN using WireGuard
- Advertise LAN subnets for routing
- DNS configuration
- Exit node capability (optional)
- Secure access to your NAS from anywhere

**Required Configuration:**
- `TS_AUTHKEY` - Get from [Tailscale Admin Console](https://login.tailscale.com/admin/settings/keys)
- `TS_STATE_DIR` - Directory to persist Tailscale state (machine key)
- `TS_ROUTES` - LAN subnet to advertise (e.g., 192.168.1.0/24)
- `TS_ACCEPT_DNS` - Accept DNS from Tailscale (recommended: true)

**Optional Configuration:**
- `TS_EXTRA_ARGS` - Additional Tailscale flags
  - `--advertise-exit-node` - Make this node available as exit node
  - `--hostname=my-nas` - Set custom hostname

**Security Features:**
- Uses specific capabilities (`NET_ADMIN`, `SYS_MODULE`) instead of privileged mode
- Host network mode for proper route management
- TUN device mapping for WireGuard tunnels

**Deployment:**

1. Copy `tailscale.yml` to Portainer
2. Configure environment variables in `.env`:
   - `TS_AUTHKEY` (required)
   - `TS_ROUTES` (adjust to your LAN subnet)
   - `TS_STATE_DIR` (directory for state persistence)
3. Deploy the stack
4. Access your NAS via Tailscale network
5. Check status: `docker exec -it tailscale tailscale status`

**Managing Routes:**

After deployment, you need to accept the routes in the Tailscale admin console:
1. Go to https://login.tailscale.com/admin/machines
2. Find your NAS device
3. Click "Edit route settings"
4. Accept the advertised routes

This allows other Tailscale devices to route through your NAS to reach your LAN.

