# YOURLS Docker Setup

This repository contains a Docker Compose configuration for running [YOURLS](https://yourls.org/) with Apache web server and MySQL database.

## Prerequisites

- Docker Desktop (or Docker Engine + Docker Compose Plugin)
- For custom domains: DNS setup pointing to your server

## Quick start

1. Copy the sample environment file and adjust values as needed:

   ```bash
   cp .env.example .env
   ```

   - `YOURLS_DOMAIN` should be just the hostname (without protocol)
   - `YOURLS_SITE` should be the full URL including protocol and port
   - `YOURLS_USER`/`YOURLS_PASS` define the admin credentials
   - `YOURLS_DB_*` values configure the MySQL database container

2. For local development, add to `/etc/hosts` (optional):

   ```bash
   sudo sh -c 'echo "127.0.0.1 short.local" >> /etc/hosts'
   ```

3. Start the stack:

   ```bash
   docker compose up -d --build
   ```

4. Open YOURLS at `http://localhost:9001/admin` and follow the setup steps.

5. After installation, test creating a short URL and verify it works.

## Testing Short URLs

After installation:
1. Go to `http://localhost:9001/admin` 
2. Create a short URL (e.g., `test` -> `https://example.com`)
3. Test the short URL: `http://localhost:9001/test`

## Architecture

- **Apache** web server with mod_rewrite for URL shortening
- **MySQL** database for storing URLs and statistics
- **Docker Compose** orchestration with health checks
- **Persistent volumes** for data storage

## File overview

- `docker-compose.yml` – main services configuration
- `docker-compose.prod.yml` – production overrides
- `.env.example` – sample environment configuration
- `docker/yourls/Dockerfile` – custom Apache configuration
- `docker/yourls/apache/` – Apache configuration files

## Production Deployment

See `PRODUCTION.md` for detailed production setup instructions.

## Maintenance

- View logs: `docker compose logs -f yourls`
- Stop services: `docker compose down`
- Update: `docker compose pull && docker compose up -d --build`
- Backup: Volumes `yourls_data` and `yourls_db_data` contain all data
