# Quick Start Guide

## 🚀 Quick Setup (Microservices – Docker)

### 1. Prerequisites
- Docker
- Docker Compose

---

### 2. Clone the Repository
```bash
git clone https://github.com/SalunkeYash/image-import-system.git
cd image-import-system
```

---

### 3. Environment Variables

Create `.env` inside:
```
services/api-gateway/app/.env
```

Example:
```env
GOOGLE_API_KEY=your-google-api-key
STORAGE_PROVIDER=azure
AZURE_STORAGE_CONNECTION_STRING=your-connection-string
AZURE_STORAGE_CONTAINER_NAME=images
```

Copy the same `.env` file to:
```
services/import-service/app/
services/metadata-service/app/
services/storage-service/app/
services/worker-service/app/
```

---

### 4. Start All Services
```bash
docker compose build --no-cache
docker compose up -d
```

---

### 5. Verify
```bash
curl http://localhost:5000/api/stats
```

---

## 🧪 Test Import
```bash
curl -X POST http://localhost:5000/api/import/google-drive \
-H "Content-Type: application/json" \
-d '{
  "folder_url": "https://drive.google.com/drive/folders/FOLDER_ID"
}'
```

---

## ⚠️ Important

The `backend/` directory is **not used** in this workflow.  
All services run from the **`services/` directory using Docker Compose**.
