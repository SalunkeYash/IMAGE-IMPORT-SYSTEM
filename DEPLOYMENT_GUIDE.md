# Deployment Guide – Azure VM + Docker (Final)

This guide explains the **final, clean deployment process** for the Image Import System on an **Azure Linux Virtual Machine** using **Docker, Docker Compose, Nginx, and HTTPS**.  
It reflects the **stable and working setup** suitable for GitHub documentation and evaluation.

---

## 1. Prerequisites

- Azure Linux VM (Ubuntu recommended)
- Ports **80**, **443**, **5000–5004** open in Azure NSG
- Domain name pointing to VM public IP (example: `static.metaltroop.cv`)

---

## 2. System Setup

### Update system and install Docker

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl start docker
sudo systemctl enable docker
```

### Add user to Docker group

```bash
sudo usermod -aG docker azureuser
exit
# Login again
```

Verify:

```bash
docker ps
```

---

## 3. Clone Repository

```bash
cd ~
git clone https://github.com/SalunkeYash/image-import-system.git
cd image-import-system
```

Verify structure:

```bash
ls
ls services
```

---

## 4. Clean Previous Docker State

```bash
docker compose down -v
docker system prune -f
```

---

## 5. Environment Configuration

### Create `.env` file

Create `.env` inside **API Gateway**:

```bash
cd services/api-gateway/app
vim .env
```

Add required variables (example):

```env
GOOGLE_API_KEY=your_key
STORAGE_PROVIDER=azure
AZURE_STORAGE_CONNECTION_STRING=your_connection_string
AZURE_STORAGE_CONTAINER_NAME=images
```

### Copy `.env` to all services

```bash
cp .env ../../import-service/
cp .env ../../metadata-service/
cp .env ../../storage-service/
cp .env ../../worker-service/
```

Move `.env` into each service’s `app/` directory:

```bash
mv services/import-service/.env services/import-service/app/
mv services/metadata-service/.env services/metadata-service/app/
mv services/storage-service/.env services/storage-service/app/
mv services/worker-service/.env services/worker-service/app/
```

---

## 6. Build and Run Containers

From project root:

```bash
docker compose build --no-cache
docker compose up -d
```

Check running containers:

```bash
docker ps
```

---

## 7. Add Swap Memory (Recommended for Small VMs)

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

Verify:

```bash
free -h
```

---

## 8. Local API Verification

```bash
curl http://localhost:5000
curl http://localhost:5000/api
curl http://localhost:5000/api/stats
```

---

## 9. Nginx Reverse Proxy Setup

### Install Nginx

```bash
sudo apt install nginx -y
```

### Create Nginx configuration

```bash
sudo vim /etc/nginx/sites-available/static.metaltroop.cv
```

```nginx
server {
    listen 80;
    server_name static.metaltroop.cv;

    location / {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/static.metaltroop.cv /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

---

## 10. Enable HTTPS (Let’s Encrypt)

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d static.metaltroop.cv
```

Verify:

```bash
curl https://static.metaltroop.cv/api/stats
```

---

## 11. Running Services

| Service | Port |
|-------|------|
| API Gateway | 5000 |
| Import Service | 5001 |
| Metadata Service | 5002 |
| Storage Service | 5003 |
| Worker Service | 5004 |

---

## 12. Final Checks

```bash
docker ps
sudo systemctl status nginx
```

---

## 13. Deployment Status

- All microservices running in Docker
- Reverse proxied via Nginx
- HTTPS enabled with Certbot
- Deployed successfully on Azure VM

---

✅ **This document is the final, clean deployment guide suitable for GitHub and project evaluation.**

