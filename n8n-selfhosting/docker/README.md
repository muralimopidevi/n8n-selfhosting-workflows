# n8n Production Docker Setup

<div align="center">

![n8n.io - Workflow Automation](https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-logo.png)

**A production-ready Docker Compose setup for self-hosting n8n with PostgreSQL, Redis, and scalable workers**

[![Docker](https://img.shields.io/badge/Docker-20.10+-blue.svg)](https://www.docker.com/)
[![n8n](https://img.shields.io/badge/n8n-1.123.28-orange.svg)](https://n8n.io/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-blue.svg)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-7-red.svg)](https://redis.io/)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Configuration](#-configuration)
- [Deployment](#-deployment)
- [Scaling Workers](#-scaling-workers)
- [Backup & Recovery](#-backup--recovery)
- [Security Best Practices](#-security-best-practices)
- [Troubleshooting](#-troubleshooting)
- [Maintenance](#-maintenance)
- [Advanced Configuration](#-advanced-configuration)

---

## 🌟 Overview

This repository provides a **production-ready Docker Compose configuration** for self-hosting [n8n](https://n8n.io/), a powerful workflow automation tool. The setup includes:

- **PostgreSQL 16** - Reliable database backend
- **Redis 7** - Queue management for distributed job processing
- **n8n Main** - Web UI, API, and webhook handler
- **n8n Workers** - Scalable background job processors
- **Health Checks** - Automated service monitoring
- **Persistent Volumes** - Data preservation across restarts
- **Logging** - Structured logs with rotation

---

## ✨ Features

### 🚀 Production-Ready
- **Queue-based architecture** with Redis for distributed processing
- **Scalable workers** - Scale horizontally by adding more worker containers
- **Health checks** for all services with automatic restarts
- **Graceful shutdown** handling for workers
- **Task runners** enabled for sandboxed Code node execution

### 🔒 Security
- **Environment-based secrets** - No hardcoded credentials
- **Password-protected Redis** connection
- **PostgreSQL** with configurable authentication
- **File permission enforcement** for n8n settings
- **Blocked environment variable access** in nodes

### 📊 Observability
- **JSON-formatted logs** with size and rotation limits
- **Health endpoints** for monitoring
- **Optional metrics** collection
- **Queue health checks** for worker status

### 🔧 Flexibility
- **Parameterized configuration** via `.env` file
- **Version pinning** for reproducible deployments
- **Optional features** (basic auth, SMTP, metrics)
- **Network isolation** with dedicated bridge network

---

## 🏗️ Architecture

### System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                       Internet / Users                           │
└────────────────────────────────┬────────────────────────────────┘
                                 │
                                 │ HTTPS (443)
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│              Reverse Proxy (Nginx/Traefik/Caddy)                │
│                     SSL/TLS Termination                          │
│                  (Not included - recommended)                    │
└────────────────────────────────┬────────────────────────────────┘
                                 │
                                 │ HTTP :5678
                                 │
                ┌────────────────┴────────────────┐
                │      Docker Network (bridge)     │
                │         n8n_network              │
                └────────────────┬────────────────┘
                                 │
                                 ▼
        ┌────────────────────────────────────────────────┐
        │              n8n-main Container                 │
        │  ┌──────────────────────────────────────────┐  │
        │  │  Web UI (React)                          │  │
        │  │  REST API                                │  │
        │  │  Webhook Endpoints                       │  │
        │  │  Workflow Editor                         │  │
        │  └──────────────────────────────────────────┘  │
        │              Port: 5678                         │
        └────┬───────────────────┬──────────────┬────────┘
             │                   │              │
             │                   │              │
   ┌─────────▼─────────┐  ┌──────▼───────┐  ┌─▼──────────┐
   │  Job Queue (Put)  │  │   Database   │  │   Cache    │
   │                   │  │   Read/Write │  │  Session   │
   └─────────┬─────────┘  └──────────────┘  └────────────┘
             │
             │
┌────────────▼────────────────────────────────────────────────────┐
│                        Redis Container                           │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │              Bull Queue (Job Queue)                        │ │
│  │  - Pending jobs                                            │ │
│  │  - Active jobs                                             │ │
│  │  - Completed jobs                                          │ │
│  │  - Failed jobs (with retry logic)                         │ │
│  └────────────────────────────────────────────────────────────┘ │
│              Port: 6379 (internal only)                          │
│              Persistence: AOF (append-only file)                 │
└────────────┬────────────────────────────────┬──────────────────┘
             │                                │
             │ Job Consume (Get)              │ Job Consume (Get)
             │                                │
     ┌───────▼──────────┐            ┌───────▼──────────┐
     │   n8n-worker-1   │            │   n8n-worker-N   │
     │ ┌──────────────┐ │            │ ┌──────────────┐ │
     │ │ Job Executor │ │   ...      │ │ Job Executor │ │
     │ │ Task Runner  │ │            │ │ Task Runner  │ │
     │ └──────┬───────┘ │            │ └──────┬───────┘ │
     │        │         │            │        │         │
     └────────┼─────────┘            └────────┼─────────┘
              │                               │
              │ Store Results                 │ Store Results
              │                               │
              └───────────┬───────────────────┘
                          │
                          ▼
        ┌─────────────────────────────────────────┐
        │        PostgreSQL Container              │
        │  ┌───────────────────────────────────┐  │
        │  │  n8n Database (public schema)     │  │
        │  │  - Users & credentials            │  │
        │  │  - Workflows                      │  │
        │  │  - Executions & history           │  │
        │  │  - Settings                       │  │
        │  └───────────────────────────────────┘  │
        │         Port: 5432 (internal only)       │
        │         Persistence: PGDATA volume       │
        └─────────────────────────────────────────┘
```

### Request Flow Diagram

```
1. User Triggers Workflow
   │
   │ HTTP Request
   ▼
┌─────────────────┐
│   n8n-main      │
│  (API Handler)  │
└────────┬────────┘
         │
         │ 1. Save workflow execution to DB
         ▼
   ┌──────────┐
   │PostgreSQL│
   └──────────┘
         │
         │ 2. Create job and push to queue
         ▼
   ┌──────────┐
   │  Redis   │
   │ (Queue)  │
   └────┬─────┘
        │
        │ 3. Worker picks job (FIFO)
        ▼
┌─────────────────┐
│  n8n-worker     │
│ (Job Executor)  │
└────────┬────────┘
         │
         │ 4. Execute workflow steps
         │    - Fetch data
         │    - Transform data
         │    - Call APIs
         │    - Run code (sandboxed)
         │
         │ 5. Store execution result
         ▼
   ┌──────────┐
   │PostgreSQL│
   └──────────┘
         │
         │ 6. Update job status
         ▼
   ┌──────────┐
   │  Redis   │
   └──────────┘
         │
         │ 7. User views result
         ▼
┌─────────────────┐
│   n8n-main      │
│  (Web UI)       │
└─────────────────┘
```

### Data Persistence

```
Host Filesystem              Docker Containers
────────────────────────────────────────────────────────
${N8N_VOLUMES_ROOT}/
│
├── postgres_data/     →    /var/lib/postgresql/data
│   ├── pgdata/             (PostgreSQL data files)
│   ├── pg_wal/             (Write-Ahead Logs)
│   └── ...
│
├── redis_data/        →    /data
│   ├── appendonly.aof      (Redis AOF persistence)
│   └── ...
│
└── n8n_data/          →    /home/node/.n8n
    ├── config/             (n8n configuration)
    ├── nodes/              (Custom nodes)
    └── ...
```

---

## 📦 Prerequisites

Before deploying n8n, ensure you have:

### Required Software
- **Docker Engine** 20.10 or higher
- **Docker Compose** v2.0 or higher
- **Linux/macOS/Windows** with WSL2 (for Windows)

### System Requirements
- **CPU**: 2+ cores (4+ recommended for production)
- **RAM**: 4GB minimum (8GB+ recommended)
- **Disk**: 20GB minimum (depends on workflow data)
- **Network**: Open port 5678 (or configure reverse proxy)

### Domain & SSL (Production)
- Domain name pointing to your server
- Reverse proxy (Nginx, Traefik, or Caddy) for HTTPS
- SSL certificate (Let's Encrypt recommended)

### Verify Installation

```bash
# Check Docker version
docker --version
# Should output: Docker version 20.10.x or higher

# Check Docker Compose version
docker compose version
# Should output: Docker Compose version v2.x.x or higher

# Test Docker functionality
docker run hello-world
```

---

## 🚀 Quick Start

### 1. Clone or Download

```bash
# If using Git
git clone <your-repo-url>
cd n8n-selfhosting/docker

# Or download and extract the ZIP file
```

### 2. Create Environment File

```bash
# Copy the example environment file
cp .env-example .env
```

### 3. Configure Environment Variables

Edit the `.env` file and update the following **required** values:

```bash
# Open with your preferred editor
nano .env
# or
vim .env
```

**Minimum Required Configuration:**

```bash
# 1. Set your volume path (where data will be stored)
N8N_VOLUMES_ROOT=/opt/n8n/volumes

# 2. Set your domain (or localhost for testing)
N8N_HOST=n8n.example.com
N8N_WEBHOOK_URL=https://n8n.example.com/

# 3. Generate encryption key (MUST be 64 hex characters)
N8N_ENCRYPTION_KEY=$(openssl rand -hex 32)

# 4. Generate PostgreSQL password
N8N_POSTGRES_PASSWORD=$(openssl rand -base64 48)

# 5. Generate Redis password
N8N_REDIS_PASSWORD=$(openssl rand -base64 48)
```

**Quick Password Generation:**

```bash
# Generate all required secrets at once
echo "N8N_ENCRYPTION_KEY=$(openssl rand -hex 32)"
echo "N8N_POSTGRES_PASSWORD=$(openssl rand -base64 48)"
echo "N8N_REDIS_PASSWORD=$(openssl rand -base64 48)"
```

### 4. Create Volumes Directory

```bash
# Create the directory for persistent data
sudo mkdir -p /opt/n8n/volumes/{postgres_data,redis_data,n8n_data}

# Set proper permissions (optional, Docker will handle this)
sudo chown -R 1000:1000 /opt/n8n/volumes
```

### 5. Start the Stack

```bash
# Start all services in detached mode
docker compose up -d

# View logs (optional)
docker compose logs -f

# Check service status
docker compose ps
```

### 6. Access n8n

Open your browser and navigate to:
- **Local testing**: `http://localhost:5678`
- **Production**: `https://your-domain.com`

**First-time setup:**
1. Create your admin account
2. Configure your first workflow
3. Test webhook functionality

---

## ⚙️ Configuration

### Environment Variables Reference

#### Deployment Paths
| Variable | Description | Example | Required |
|----------|-------------|---------|----------|
| `N8N_VOLUMES_ROOT` | Base path for all persistent volumes | `/opt/n8n/volumes` | ✅ |

#### n8n Application
| Variable | Description | Example | Required |
|----------|-------------|---------|----------|
| `N8N_VERSION` | n8n Docker image version | `1.123.28` | ✅ |
| `N8N_HOST` | Public hostname/domain | `n8n.example.com` | ✅ |
| `N8N_PORT` | Internal port (usually 5678) | `5678` | ✅ |
| `N8N_PROTOCOL` | Protocol (http/https) | `https` | ✅ |
| `N8N_WEBHOOK_URL` | Full webhook URL | `https://n8n.example.com/` | ✅ |
| `N8N_ENCRYPTION_KEY` | Database encryption key (64 hex chars) | `<generated>` | ✅ |

#### PostgreSQL Database
| Variable | Description | Example | Required |
|----------|-------------|---------|----------|
| `N8N_POSTGRES_DB` | Database name | `n8n` | ✅ |
| `N8N_POSTGRES_USER` | Database user | `n8n` | ✅ |
| `N8N_POSTGRES_PASSWORD` | Database password | `<generated>` | ✅ |

#### Redis Queue
| Variable | Description | Example | Required |
|----------|-------------|---------|----------|
| `N8N_REDIS_PASSWORD` | Redis password | `<generated>` | ✅ |

#### Performance & Scaling
| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `N8N_WORKER_CONCURRENCY` | Parallel jobs per worker | `10` | ❌ |

#### Logging & Monitoring
| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `N8N_LOG_LEVEL` | Log verbosity (error/warn/info/debug) | `info` | ❌ |
| `N8N_LOG_OUTPUT` | Log output destination | `console` | ❌ |
| `N8N_METRICS` | Enable Prometheus metrics | `false` | ❌ |
| `N8N_GENERIC_TIMEZONE` | Timezone for scheduling | `UTC` | ❌ |

#### Optional: Basic Authentication
| Variable | Description | Example | Required |
|----------|-------------|---------|----------|
| `N8N_BASIC_AUTH_ACTIVE` | Enable basic auth | `true` | ❌ |
| `N8N_BASIC_AUTH_USER` | Basic auth username | `admin` | ❌ |
| `N8N_BASIC_AUTH_PASSWORD` | Basic auth password | `<strong-password>` | ❌ |

---

## 🚢 Deployment

### Standard Deployment

```bash
# Start all services
docker compose up -d

# Verify all containers are running
docker compose ps
```

Expected output:
```
NAME                IMAGE                         STATUS
n8n_main            docker.n8n.io/n8nio/n8n:...   Up (healthy)
n8n_postgres        postgres:16-alpine            Up (healthy)
n8n_redis           redis:7-alpine                Up (healthy)
n8n-worker-1        docker.n8n.io/n8nio/n8n:...   Up (healthy)
n8n-worker-2        docker.n8n.io/n8nio/n8n:...   Up (healthy)
n8n-worker-3        docker.n8n.io/n8nio/n8n:...   Up (healthy)
```

### View Logs

```bash
# All services
docker compose logs -f

# Specific service
docker compose logs -f n8n-main
docker compose logs -f n8n-worker

# Last 100 lines
docker compose logs --tail=100 n8n-main
```

### Stop Services

```bash
# Stop all services (keeps data)
docker compose down

# Stop and remove volumes (DELETES ALL DATA!)
docker compose down -v
```

---

## 📈 Scaling Workers

The n8n worker service is designed for horizontal scaling to handle increased workload.

### Current Configuration

By default, the setup starts **3 worker instances**:

```yaml
n8n-worker:
  deploy:
    replicas: 3
```

### Scale Workers Dynamically

```bash
# Scale to 5 workers
docker compose up -d --scale n8n-worker=5

# Scale down to 1 worker
docker compose up -d --scale n8n-worker=1

# Verify worker count
docker compose ps n8n-worker
```

### When to Scale?

**Scale UP when:**
- Job queue is consistently backed up
- Workflow execution times are increasing
- Redis queue has many pending jobs
- CPU usage across workers is consistently high

**Scale DOWN when:**
- Workers are mostly idle
- Queue is consistently empty
- Resource costs need reduction

### Monitoring Worker Performance

```bash
# Check Redis queue status (inside Redis container)
docker compose exec redis redis-cli -a "${N8N_REDIS_PASSWORD}" INFO stats

# Monitor worker CPU/memory usage
docker stats
```

### Worker Concurrency

Each worker processes multiple jobs simultaneously, controlled by `N8N_WORKER_CONCURRENCY`:

```bash
# In .env file
N8N_WORKER_CONCURRENCY=10  # Each worker handles 10 concurrent jobs
```

**Total concurrent jobs** = `Number of Workers × Worker Concurrency`

Example: 3 workers × 10 concurrency = 30 simultaneous workflow executions

---

## 💾 Backup & Recovery

### What to Backup

1. **PostgreSQL Database** - Workflows, executions, credentials
2. **n8n Data Directory** - Settings, custom nodes
3. **Environment File** - Configuration (store securely)

### Automated PostgreSQL Backup

```bash
#!/bin/bash
# backup-n8n.sh

BACKUP_DIR="/backup/n8n"
DATE=$(date +%Y%m%d_%H%M%S)
CONTAINER="n8n_postgres"

# Create backup directory
mkdir -p ${BACKUP_DIR}

# Backup database
docker compose exec -T postgres pg_dump -U n8n n8n | gzip > ${BACKUP_DIR}/n8n_db_${DATE}.sql.gz

# Backup n8n data directory
tar -czf ${BACKUP_DIR}/n8n_data_${DATE}.tar.gz /opt/n8n/volumes/n8n_data/

# Keep only last 7 days of backups
find ${BACKUP_DIR} -name "*.gz" -mtime +7 -delete

echo "Backup completed: ${DATE}"
```

**Make executable and schedule:**

```bash
chmod +x backup-n8n.sh

# Add to crontab (daily at 2 AM)
crontab -e
0 2 * * * /path/to/backup-n8n.sh >> /var/log/n8n-backup.log 2>&1
```

### Manual Backup

```bash
# Stop n8n services (workers and main)
docker compose stop n8n-main n8n-worker

# Backup PostgreSQL
docker compose exec postgres pg_dump -U n8n n8n > n8n_backup_$(date +%Y%m%d).sql

# Backup n8n data
tar -czf n8n_data_$(date +%Y%m%d).tar.gz /opt/n8n/volumes/n8n_data/

# Restart services
docker compose start n8n-main n8n-worker
```

### Restore from Backup

```bash
# Stop all services
docker compose down

# Remove old data (CAUTION!)
sudo rm -rf /opt/n8n/volumes/postgres_data/*
sudo rm -rf /opt/n8n/volumes/n8n_data/*

# Start only database
docker compose up -d postgres

# Wait for PostgreSQL to be ready
sleep 10

# Restore database
gunzip -c n8n_db_YYYYMMDD.sql.gz | docker compose exec -T postgres psql -U n8n -d n8n

# Restore n8n data
tar -xzf n8n_data_YYYYMMDD.tar.gz -C /

# Start all services
docker compose up -d
```

---

## 🔒 Security Best Practices

### 1. Use Strong Passwords

```bash
# Generate cryptographically secure passwords
openssl rand -base64 48
```

### 2. Enable Basic Authentication (if no reverse proxy)

In `.env`:
```bash
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=<strong-password>
```

### 3. Use HTTPS (Reverse Proxy)

**Nginx Example:**

```nginx
server {
    listen 80;
    server_name n8n.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name n8n.example.com;

    ssl_certificate /etc/letsencrypt/live/n8n.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/n8n.example.com/privkey.pem;

    location / {
        proxy_pass http://localhost:5678;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # WebSocket support
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

### 4. Firewall Configuration

```bash
# Ubuntu/Debian with ufw
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

# Block direct access to n8n port (if using reverse proxy)
sudo ufw deny 5678/tcp
```

### 5. Regular Updates

```bash
# Update n8n version in .env
N8N_VERSION=1.xxx.xx

# Pull new image and restart
docker compose pull
docker compose up -d
```

### 6. Environment File Security

```bash
# Restrict .env file permissions
chmod 600 .env
chown root:root .env

# Never commit .env to version control
echo ".env" >> .gitignore
```

### 7. Network Isolation

The docker-compose.yml already uses a dedicated network (`n8n_network`), isolating services from other containers.

### 8. Limit Resource Usage

Add resource limits to prevent DoS:

```yaml
# Add to docker-compose.yml under each service
deploy:
  resources:
    limits:
      cpus: '2'
      memory: 2G
    reservations:
      memory: 1G
```

---

## 🔧 Troubleshooting

### Service Won't Start

**Check logs:**
```bash
docker compose logs -f
```

**Common issues:**
- Missing or invalid `.env` file
- Incorrect permissions on volumes directory
- Port 5678 already in use
- Docker daemon not running

**Solution:**
```bash
# Check if port is in use
sudo netstat -tlnp | grep 5678

# Verify .env exists and is readable
ls -la .env

# Check Docker status
sudo systemctl status docker
```

### Database Connection Errors

**Symptoms:** n8n-main shows "Cannot connect to database"

**Check PostgreSQL health:**
```bash
docker compose ps postgres
docker compose logs postgres
```

**Solution:**
```bash
# Restart PostgreSQL
docker compose restart postgres

# Wait for health check
docker compose ps postgres

# If still failing, check password in .env matches docker-compose.yml
```

### Redis Connection Errors

**Symptoms:** Workers can't connect to queue

**Check Redis:**
```bash
docker compose logs redis

# Test Redis connection
docker compose exec redis redis-cli -a "${N8N_REDIS_PASSWORD}" ping
# Should return: PONG
```

**Solution:**
```bash
docker compose restart redis
```

### Workers Not Processing Jobs

**Check worker status:**
```bash
docker compose ps n8n-worker
docker compose logs -f n8n-worker
```

**Verify queue:**
```bash
docker compose exec redis redis-cli -a "${N8N_REDIS_PASSWORD}" LLEN bull:n8n:jobs:wait
```

**Solution:**
```bash
# Restart workers
docker compose restart n8n-worker

# Increase worker count
docker compose up -d --scale n8n-worker=5
```

### Webhooks Not Working

**Check webhook URL:**
```bash
# In n8n UI: Settings > n8n Settings
# Verify: Webhook URL = https://your-domain.com/
```

**Common issues:**
- `N8N_WEBHOOK_URL` doesn't match actual domain
- Reverse proxy not forwarding requests
- Firewall blocking incoming webhooks

### Permission Denied Errors

```bash
# Fix volume permissions
sudo chown -R 1000:1000 /opt/n8n/volumes/n8n_data
sudo chown -R 999:999 /opt/n8n/volumes/postgres_data
```

### High Memory Usage

**Monitor resource usage:**
```bash
docker stats
```

**Solutions:**
- Reduce `N8N_WORKER_CONCURRENCY`
- Scale down workers
- Add memory limits in docker-compose.yml
- Increase server RAM

### Slow Performance

**Possible causes:**
- Too many concurrent workflows
- Insufficient workers
- Database not optimized
- Low server resources

**Solutions:**
```bash
# Scale up workers
docker compose up -d --scale n8n-worker=5

# Increase concurrency
# In .env: N8N_WORKER_CONCURRENCY=15

# Restart services
docker compose restart
```

---

## 🛠️ Maintenance

### Update n8n Version

```bash
# 1. Backup first!
./backup-n8n.sh

# 2. Update version in .env
# Change: N8N_VERSION=1.xxx.xx

# 3. Pull new image
docker compose pull

# 4. Restart services
docker compose up -d

# 5. Verify
docker compose ps
docker compose logs -f n8n-main
```

### Clean Up Old Images

```bash
# Remove unused Docker images
docker image prune -a

# Remove old n8n images
docker images | grep n8n | grep -v $(grep N8N_VERSION .env | cut -d'=' -f2)
```

### Database Maintenance

```bash
# Vacuum PostgreSQL (reclaim storage)
docker compose exec postgres vacuumdb -U n8n -d n8n -v -z

# Check database size
docker compose exec postgres psql -U n8n -d n8n -c "SELECT pg_size_pretty(pg_database_size('n8n'));"
```

### Log Rotation

Logs are automatically rotated by Docker (see docker-compose.yml):
```yaml
logging:
  driver: "json-file"
  options:
    max-size: "50m"
    max-file: "10"
```

**Manual cleanup:**
```bash
# Clear all Docker logs
sudo sh -c 'truncate -s 0 /var/lib/docker/containers/*/*-json.log'
```

---

## 🚀 Advanced Configuration

### Add Reverse Proxy (Traefik)

```yaml
# Add to docker-compose.yml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.n8n.rule=Host(`n8n.example.com`)"
  - "traefik.http.routers.n8n.entrypoints=websecure"
  - "traefik.http.routers.n8n.tls.certresolver=letsencrypt"
  - "traefik.http.services.n8n.loadbalancer.server.port=5678"
```

### Enable Metrics (Prometheus)

In `.env`:
```bash
N8N_METRICS=true
```

**Scrape metrics:**
```
http://your-n8n:5678/metrics
```

### Custom Node Installation

```bash
# Install custom nodes in n8n container
docker compose exec n8n-main npm install n8n-nodes-custom-package

# Restart to load
docker compose restart n8n-main n8n-worker
```

### Email Notifications (SMTP)

In `.env`:
```bash
N8N_EMAIL_MODE=smtp
N8N_SMTP_HOST=smtp.gmail.com
N8N_SMTP_PORT=587
N8N_SMTP_USER=your-email@gmail.com
N8N_SMTP_PASS=your-app-password
N8N_SMTP_SENDER=n8n@example.com
```

---

## 📚 Additional Resources

- **n8n Documentation**: https://docs.n8n.io/
- **n8n Community Forum**: https://community.n8n.io/
- **Docker Documentation**: https://docs.docker.com/
- **PostgreSQL Documentation**: https://www.postgresql.org/docs/
- **Redis Documentation**: https://redis.io/documentation

---

## 📄 License

This configuration is provided as-is under the MIT License. n8n itself is licensed under the [Sustainable Use License](https://github.com/n8n-io/n8n/blob/master/LICENSE.md).

---

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Test your changes thoroughly
4. Submit a pull request

---

## ⚠️ Disclaimer

This setup is provided as a starting point for self-hosting n8n. Always:
- Test in a non-production environment first
- Keep regular backups
- Monitor your infrastructure
- Keep software updated
- Follow security best practices

---

<div align="center">

**Made with ❤️ for the n8n community**

If this setup helped you, consider ⭐ starring the repository!

</div>
