# 🚀 WhatsApp Evolution API Installation Guide

Complete step-by-step guide to install and configure the WhatsApp Evolution API for your n8n workflows.

## 📋 What is Evolution API?

Evolution API is an open-source WhatsApp integration API that provides a comprehensive platform for messaging automation. It supports multiple messaging services and integrations, making it perfect for n8n workflows.

**Key Features:**
- 📱 WhatsApp Web API (Baileys-based) - Free
- ☁️ WhatsApp Cloud API (Official Meta API) - Premium
- 🤖 Multiple integrations: Typebot, Chatwoot, Dify, OpenAI
- 🔗 Perfect for n8n automation workflows
- 📊 Real-time messaging and media handling

**Official Repository:** [Evolution API on GitHub](https://github.com/EvolutionAPI/evolution-api)

## 🛠️ Prerequisites

Before starting the installation, ensure you have:

- ✅ **Docker Desktop** installed and running
- ✅ **WSL2** enabled (for Windows)
- ✅ **Command Prompt or PowerShell** access
- ✅ **Ports 8080, 5432, and 6379** available

### 🪟 **Why WSL2 is Required for Docker on Windows**

Docker Desktop on Windows uses WSL2 as its backend because:
- **🐧 Linux Containers:** Docker containers are Linux-based, WSL2 provides the Linux kernel
- **⚡ Performance:** WSL2 offers near-native Linux performance vs slower Hyper-V
- **🔗 Integration:** Seamless file sharing between Windows and Linux environments
- **💾 Resource Efficiency:** Better memory and CPU usage than traditional VMs

### **Installing WSL2 on Windows**

**One-command install:**

1. Open **PowerShell as Administrator** (right-click → Run as administrator)
2. Run: `wsl --install`
3. Restart when prompted

This enables everything and installs Ubuntu by default.

## 🚀 Complete Evolution API Installation - Working Solution

### Step 1: Complete Cleanup (if needed)

```cmd
docker stop evolution_api evolution_postgres evolution_redis
docker rm -f evolution_api evolution_postgres evolution_redis
docker system prune -f
```

### Step 2: Start PostgreSQL Database

```cmd
docker run -d --name evolution_postgres -e POSTGRES_DB=evolution -e POSTGRES_USER=evolution -e POSTGRES_PASSWORD=evolution123 -p 5432:5432 postgres:13
```

### Step 3: Start Redis Cache

```cmd
docker run -d --name evolution_redis -p 6379:6379 redis:7-alpine
```

### Step 4: Wait for Services to Initialize

**Check PostgreSQL:**
```cmd
docker logs evolution_postgres
```
Wait for: `"database system is ready to accept connections"`

**Check Redis:**
```cmd
docker logs evolution_redis
```
Should show: `"Ready to accept connections"`

### Step 5: Start Evolution API with QR Code Fix

```cmd
docker stop evolution_api && docker rm evolution_api && docker run -d --name evolution_api -p 8080:8080 -e AUTHENTICATION_API_KEY=EvolutionAPI_MySecure_Key_2024_789xyz -e DATABASE_PROVIDER=postgresql -e DATABASE_CONNECTION_URI=postgresql://evolution:evolution123@host.docker.internal:5432/evolution -e REDIS_URI=redis://host.docker.internal:6379 -e SERVER_URL=http://localhost:8080 -e CORS_ORIGIN=* -e DATABASE_ENABLED=true -e DATABASE_SAVE_MESSAGE_UPDATE=true -e DATABASE_SAVE_DATA_CHATS=true -e CONFIG_SESSION_PHONE_VERSION=2.3000.1023181082 --link evolution_postgres:postgres --link evolution_redis:redis atendai/evolution-api:latest
```

### Step 6: Verify All Services Running

```cmd
docker ps
```

**Expected output: 3 containers running**
- `evolution_postgres`
- `evolution_redis`  
- `evolution_api`

### Step 7: Check Evolution API Logs

```cmd
docker logs evolution_api
```

**Should see:**
- ✅ Database migrations completed
- ✅ HTTP - ON: 8080
- ✅ No Redis disconnect errors
- ✅ WA MODULE - ON

### Step 8: Verify API is Working

```cmd
curl http://localhost:8080
```

## 🔧 Configuration

### API Key Setup

Your Evolution API is secured with the key: `EvolutionAPI_MySecure_Key_2024_789xyz`

**Important Security Notes:**
- 🔐 Change the default API key in production
- 🚫 Never expose your API key publicly
- 🔒 Use environment variables for sensitive data

### Access Points

- **API Base:** `http://localhost:8080`
- **Manager Interface:** `http://localhost:8080/manager`
- **API Documentation:** `http://localhost:8080/docs`

## 💻 **All Commands Summary**

Copy and paste these commands in order:

### **1. Install WSL2 (Windows Only)**
```cmd
# Open PowerShell as Administrator
wsl --install
# Restart computer when prompted
```

### **2. Complete Cleanup (if needed)**
```cmd
docker stop evolution_api evolution_postgres evolution_redis
docker rm -f evolution_api evolution_postgres evolution_redis
docker system prune -f
```

### **3. Start PostgreSQL Database**
```cmd
docker run -d --name evolution_postgres -e POSTGRES_DB=evolution -e POSTGRES_USER=evolution -e POSTGRES_PASSWORD=evolution123 -p 5432:5432 postgres:13
```

### **4. Start Redis Cache**
```cmd
docker run -d --name evolution_redis -p 6379:6379 redis:7-alpine
```

### **5. Wait for Services (Check Logs)**
```cmd
docker logs evolution_postgres
docker logs evolution_redis
```

### **6. Start Evolution API with QR Code Fix**
```cmd
docker stop evolution_api && docker rm evolution_api && docker run -d --name evolution_api -p 8080:8080 -e AUTHENTICATION_API_KEY=EvolutionAPI_MySecure_Key_2024_789xyz -e DATABASE_PROVIDER=postgresql -e DATABASE_CONNECTION_URI=postgresql://evolution:evolution123@host.docker.internal:5432/evolution -e REDIS_URI=redis://host.docker.internal:6379 -e SERVER_URL=http://localhost:8080 -e CORS_ORIGIN=* -e DATABASE_ENABLED=true -e DATABASE_SAVE_MESSAGE_UPDATE=true -e DATABASE_SAVE_DATA_CHATS=true -e CONFIG_SESSION_PHONE_VERSION=2.3000.1023181082 --link evolution_postgres:postgres --link evolution_redis:redis atendai/evolution-api:latest
```

### **7. Verify Installation**
```cmd
docker ps
docker logs evolution_api
curl http://localhost:8080
```

**🎯 Your Evolution API will be running at:** `http://localhost:8080`

