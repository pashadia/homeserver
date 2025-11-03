# Nextcloud Docker Compose Stack

This directory contains a Docker Compose configuration for running Nextcloud with PostgreSQL database and Valkey (Redis-compatible) caching.

## Components

- **Nextcloud**: Main application with Apache web server
- **PostgreSQL**: Database server (dedicated to this Nextcloud instance)
- **Valkey**: Redis-compatible caching and session storage

## Prerequisites

1. Ensure Docker/Podman and Docker Compose are installed
2. Create the required host directories:
   ```bash
   sudo mkdir -p /data/nextcloud/{db,html,data}
   sudo chown -R 33:33 /data/nextcloud/html /data/nextcloud/data  # www-data user
   ```

## Installation

1. **Configure environment variables:**
   ```bash
   cp .env.example .env
   ```

2. **Edit `.env` file with secure passwords:**
   - Set `POSTGRES_PASSWORD` to a strong database password
   - Set `VALKEY_PASSWORD` to a strong cache password
   - Set `NEXTCLOUD_ADMIN_PASSWORD` to a strong admin password
   - Update `NEXTCLOUD_TRUSTED_DOMAINS` with your server's IP address

3. **Deploy the stack:**
   ```bash
   docker compose up -d
   ```

4. **Access Nextcloud:**
   - Open http://your-server-ip in your browser
   - Login with the admin credentials from your `.env` file

## Storage Structure

```
/data/nextcloud/
├── db/            # PostgreSQL data
├── html/          # Nextcloud application files, config, apps, themes
└── data/          # User uploaded files and data
```

## Configuration Notes

- Nextcloud is configured to use PostgreSQL as database
- Valkey handles caching and session storage for better performance
- File uploads are limited to 10GB (configurable via PHP_UPLOAD_LIMIT)
- PHP memory limit is set to 512MB
- Currently configured for HTTP only (HTTPS/SSL can be added later with Traefik)

## Maintenance

- **Updates**: Pull new images and restart containers
- **Backups**: Back up `/data/nextcloud/` directory
- **Logs**: View with `docker compose logs -f [service-name]`

## Security

- All passwords are stored in environment variables (never in compose file)
- Database is isolated to this stack only
- Services communicate via internal Docker network
