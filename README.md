## 👋 Welcome to hoarder 🚀

Self-hosted bookmark manager with full-text search and AI tagging

## 📋 Description

Hoarder (upstream name: Karakeep) is a self-hosted bookmark manager. It stores
links, notes, and images, archives the page content, and makes everything
searchable with full-text search. Optional AI features auto-tag and summarize
saved items.

## 🚀 Services

- **app**: ghcr.io/karakeep-app/karakeep:latest

### Infrastructure Components

- **hoarder-chrome**: zenika/alpine-chrome:latest
- **hoarder-meilisearch**: getmeili/meilisearch:latest - Full-text search index


## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/hoarder/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/hoarder" ~/.local/srv/docker/hoarder
cd ~/.local/srv/docker/hoarder
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install hoarder
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_SECRET_KEY=replace-with-a-random-32-byte-hex-string
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64198` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64198

The first visit prompts you to create an account. The headless Chrome and
Meilisearch services are internal-only and never published to the host.

## 📂 Volumes

- `./volumes/data/hoarder` - Bookmarks, notes, and archived content
- `./volumes/data/meilisearch/hoarder` - Search index

## 🔐 Security

- Change all default passwords before deploying to production
- The headless Chrome container renders untrusted pages — it is isolated on the
  project network with no published ports
- Meilisearch is internal-only and never published to the host
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
