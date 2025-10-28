# Backup Services

Backup solutions for protecting your homelab data.

## Available Stacks

### Duplicati

Cloud backup solution supporting multiple storage providers (S3, OneDrive, Google Drive, Backblaze, etc.).

**File:** `duplicati.yml`

**Supported Destinations:**
- Amazon S3
- Microsoft OneDrive
- Google Drive
- Backblaze B2
- Dropbox
- Azure Blob Storage
- And many more

**Features:**
- Encrypted backups
- Incremental backups
- Deduplication
- Scheduling and automation
- Web-based UI with authentication
- Optional database encryption at rest
- Read-only source mounts for safety
- Logging limits to prevent disk fill
- Security hardening with no-new-privileges

**Configuration:**

1. Deploy the stack with Portainer
2. Access web UI at `http://your-nas-ip:8200`
3. Login with credentials from `.env` file
4. Create a new backup job
5. Configure:
   - Source data to backup
   - Destination storage provider
   - Backup schedule
   - Retention policies
   - Encryption options

**Required Environment Variables:**
- `DUPLICATI__WEBSERVICE_USERNAME` - Web UI username
- `DUPLICATI__WEBSERVICE_PASSWORD` - Web UI password (set strong!)
- `BACKUP_SOURCE` - Directory to backup (read-only mount)
- `PUID` / `PGID` - User/Group IDs for proper permissions

**Optional Environment Variables:**
- `SETTINGS_ENCRYPTION_KEY` - Encrypt Duplicati's settings database
- `BACKUP_DIR` - Local backup target directory

**Important Paths:**

- Configuration: `$CONFIG_DIR/duplicati/config`
- Local Backups: `$BACKUP_DIR/duplicati/backups`
- Source Data: Set via `BACKUP_SOURCE` environment variable (read-only)
- Logs: Limited to 10MB per file, 3 files max

**Security:**
- Web UI requires authentication (configure in .env)
- Source volumes mounted read-only for safety
- Database encryption at rest (optional)
- Security hardening with `no-new-privileges`
- Log rotation to prevent disk fill
- Use strong passwords for all accounts
- Store credentials securely in .env file
- Test restore procedures regularly

## Deployment

1. Copy `duplicati.yml` to Portainer
2. Create a `.env` file with:
   - `DUPLICATI__WEBSERVICE_USERNAME` (Web UI username)
   - `DUPLICATI__WEBSERVICE_PASSWORD` (Web UI password - use a strong password!)
   - `BACKUP_SOURCE` (directory to backup)
   - `PUID` / `PGID` (match your NAS user)
   - Optional: `SETTINGS_ENCRYPTION_KEY` (to encrypt the database)
3. Deploy the stack
4. Access web UI at `http://your-nas-ip:8200` and login
5. Configure your backup jobs with storage provider credentials

## Best Practices

- ✅ Run incremental backups daily
- ✅ Keep multiple restore points
- ✅ Test restore procedures monthly
- ✅ Encrypt sensitive backups
- ✅ Monitor backup success/failures
- ✅ Store encryption keys securely offline
- ✅ Consider off-site backups for critical data

## Example Backup Configuration

After deployment, in the Duplicati UI:

1. **Destination**: Configure your storage provider credentials
2. **Source**: Select `/source` volume or add additional volumes
3. **Schedule**: Set daily/weekly automated backups
4. **Options**: Configure compression, encryption, and retention

