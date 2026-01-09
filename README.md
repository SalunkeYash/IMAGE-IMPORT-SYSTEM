# Scalable Image Import System

A production-ready backend system built with Flask that imports images from public Google Drive folders and stores them in cloud object storage (AWS S3 or Azure Blob Storage) with metadata persistence.

> ⚠️ **Architecture Note**  
> This project uses a **microservices architecture**.  
> The `backend/` directory is **legacy** and kept only for reference.  
> All active development and deployment use the **Dockerized services inside the `services/` directory**.

## 🚀 Features

### Backend
- **Microservices Architecture**: System is divided into independent services
- **Multi-Cloud Storage Support**: AWS S3 and Azure Blob Storage
- **Google Drive Integration**: Import images from public Google Drive folders
- **Metadata Service**: Persistent image metadata storage
- **RESTful APIs**: Clean, well-documented endpoints
- **Scalable Design**: Services can be scaled independently
- **Environment-based Configuration**: Secure credential management

### Frontend
- **React Application**: Modern, responsive UI
- **Import Management**: Google Drive folder URL input
- **Image Gallery**: Display imported images with metadata
- **Real-time Feedback**: Loading states and error messages

## 📋 Prerequisites

- Python
- Docker
- Docker Compose
- Azure / AWS account for storage
- Google Drive API key
- Azure SQL Database 

## 🛠️ Installation

### Backend (Microservices – Recommended)

1. **Clone the repository**
```bash
git clone https://github.com/SalunkeYash/image-import-system.git
cd image-import-system
```

2. **Configure Environment Variables**

Create a `.env` file inside:
```
services/api-gateway/app/.env
```

3. **Build and run services**
```bash
docker compose build --no-cache
docker compose up -d
```

The API Gateway runs on `http://localhost:5000`

> ℹ️ The legacy `backend/` folder is **not required** for running the system.

### Frontend Setup

```bash
cd frontend
npm install
npm start
```

The frontend runs on `https://image-import.metaltroop.cv/`

## 🏗️ Architecture

```
Frontend (React)
        |
        v
API Gateway (5000)
        |
 ┌──────┼──────────────┐
 |      |              |
 v      v              v
Import  Metadata     Storage
(5001)  (5002)        (5003)
        |
        v
Worker Service (5004)
```

## 🚀 Deployment

See `DEPLOYMENT_GUIDE.md` for Azure VM deployment using Docker, Nginx, and HTTPS.

## 🛠️ Technology Stack

### Backend
- Python (Flask microservices)
- Docker & Docker Compose
- Redis
- Azure SQL / SQLite

### Frontend
- React
- Axios

## 📝 License

MIT License
