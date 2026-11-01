## 👋 Welcome to Stalwart Mail Server 🚀

All-in-one mail server with JMAP, IMAP, SMTP and a web admin

## 📋 Description

Stalwart is a single-binary mail server that speaks SMTP, IMAP, JMAP and
ManageSieve from one container. This stack runs the official image with its
configuration under `./volumes/config/stalwart` and its mail store under
`./volumes/data/stalwart`. The web admin listens on port 8080. The default compose file publishes only
that web port; mail protocol ports are not exposed by this fleet convention.

## 🚀 Services

- **app**: stalwartlabs/mail-server:latest

### Infrastructure Components

Stalwart needs no external database, Redis or queue. Configuration and the
mail store live in bind mounts on the host.

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/stalwart/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/stalwart" ~/.local/srv/docker/stalwart
cd ~/.local/srv/docker/stalwart
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install stalwart
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
BASE_HOST_NAME=mail.example.com
APP_ADMIN_USER=admin
APP_ADMIN_PASS=replace-with-a-random-password
```

`BASE_HOST_NAME` is the mail server FQDN, it must match your MX record and
the certificate served on 993.

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
| `docker-compose.yaml` | Default, web admin published on `172.17.0.1:64352` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Only the web admin goes through a reverse proxy. SMTP, IMAP and ManageSieve
are not HTTP, so the proxy variants have no `ports:` section at all. Mail
protocol access therefore requires a separate deployment or port mapping.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64352

## 📂 Volumes

- `./volumes/config/stalwart` - Stalwart configuration, mounted at
  `/opt/stalwart-mail/etc`
- `./volumes/data/stalwart` - Mail store, blobs and queue, mounted at
  `/opt/stalwart-mail/data`

## 🔐 Security

- Change `APP_ADMIN_PASS` before the first start, it is used for the account
  recovery token
- `BASE_HOST_NAME` must match the DNS name on your certificate, STARTTLS on
  587 fails otherwise
- The web admin is published on `172.17.0.1` only, it is not reachable from
  the network unless you add a reverse proxy
- Set SPF, DKIM and DMARC for the mail domain, Stalwart generates DKIM keys
  but does not publish DNS records for you
- Regularly update Docker images for security patches
- Back up `./volumes/data/stalwart`, it holds every message and the queue

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
- Inbound ports 25, 143, 465, 587 and 993 reachable from the internet
- A DNS A record for the mail FQDN and an MX record pointing at it

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
