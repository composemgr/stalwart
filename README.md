## 👋 Welcome to stalwart 🚀

All-in-one email server (SMTP, IMAP, JMAP)

## 📋 Description

All-in-one email server (SMTP, IMAP, JMAP)

## 🚀 Services

- **stalwart**: stalwartlabs/mail-server:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/stalwart/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/stalwart" ~/.local/srv/docker/stalwart
cd ~/.local/srv/docker/stalwart
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install stalwart
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
```

See `docker-compose.yaml` for complete list of configurable options.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:8091

## 📂 Volumes

- `./rootfs/config/stalwart` - Data storage
- `./rootfs/data/stalwart` - Data storage

## 🔍 Logging

```shell
docker compose logs -f stalwart
```

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# Update to latest images
docker compose pull && docker compose up -d

# View logs
docker compose logs -f

# Restart services
docker compose restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
