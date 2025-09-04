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
- ✅ **WSL2** enabled (for Windows users)
- ✅ **Git** installed
- ✅ Basic command line knowledge
- ✅ **n8n instance** running (for integration)

### 🪟 **Installing WSL2 on Windows**

**Open Command Prompt as Administrator** and run:

```cmd
# Enable WSL2 features
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# Restart computer, then run:
wsl --set-default-version 2
wsl --install -d Ubuntu

# Verify installation
wsl --list --verbose
```

**Download WSL2 kernel update:** https://aka.ms/wsl2kernel

## 🧹 Clean Up Previous Installation (If Any)

If you have a previous Evolution API installation, clean it up first:

```bash
# Stop and remove existing containers
docker stop evolution_api evolution_postgres
docker rm -f evolution_api evolution_postgres

# Remove any existing volumes (optional - only if you want to start completely fresh)
docker volume prune
```

## 🚀 Complete Fresh Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/EvolutionAPI/evolution-api.git
cd evolution-api
```

**Expected Output:**
```
Cloning into 'evolution-api'...
remote: Enumerating objects: XXXX, done.
remote: Total XXXX (delta 0), reused 0 (delta 0), pack-reused XXXX
Receiving objects: 100% (XXXX/XXXX), XX.XX MiB | XX.XX MiB/s, done.
```

### Step 2: Create PostgreSQL Database

```bash
docker run -d --name evolution_postgres \
  -e POSTGRES_DB=evolution \
  -e POSTGRES_USER=evolution \
  -e POSTGRES_PASSWORD=evolution123 \
  -p 5432:5432 \
  postgres:13
```

**Expected Output:**
```
Unable to find image 'postgres:13' locally
13: Pulling from library/postgres
...
Status: Downloaded newer image for postgres:13
[CONTAINER_ID]
```

### Step 3: Wait for PostgreSQL to Initialize

```bash
docker logs evolution_postgres
```

**Wait for this message:**
```
database system is ready to accept connections
```

⏱️ **Note:** This may take 30-60 seconds for first-time setup.

### Step 4: Install Evolution API

```bash
docker run -d --name evolution_api \
  -p 8080:8080 \
  -e AUTHENTICATION_API_KEY=EvolutionAPI_MySecure_Key_2024_789xyz \
  -e DATABASE_PROVIDER=postgresql \
  -e DATABASE_CONNECTION_URI=postgresql://evolution:evolution123@host.docker.internal:5432/evolution \
  --link evolution_postgres:postgres \
  atendai/evolution-api:latest
```

**Expected Output:**
```
Unable to find image 'atendai/evolution-api:latest' locally
latest: Pulling from atendai/evolution-api
...
Status: Downloaded newer image for atendai/evolution-api:latest
[CONTAINER_ID]
```

### Step 5: Verify Installation

```bash
# Check if containers are running
docker ps

# Check Evolution API logs
docker logs evolution_api

# Test API endpoint
curl http://localhost:8080
```

**Expected Results:**
- Both containers should be listed as "Up"
- API should respond with a welcome message or API documentation
- No critical errors in logs

## 🔧 Configuration

### API Key Setup

Your Evolution API is secured with the key: `EvolutionAPI_MySecure_Key_2024_789xyz`

**Important Security Notes:**
- 🔐 Change the default API key in production
- 🚫 Never expose your API key publicly
- 🔒 Use environment variables for sensitive data

### Access Points

- **API Base URL:** `http://localhost:8080`
- **Documentation:** `http://localhost:8080/docs` (if available)
- **Database:** PostgreSQL on port `5432`

## 🔗 Integration with n8n

### Step 1: Configure n8n HTTP Request Node

1. **Create HTTP Request Node** in your n8n workflow
2. **Set URL:** `http://localhost:8080/instance/create`
3. **Add Headers:**
   ```
   Authorization: Bearer EvolutionAPI_MySecure_Key_2024_789xyz
   Content-Type: application/json
   ```

### Step 2: Create WhatsApp Instance

**POST Request Body:**
```json
{
  "instanceName": "my_whatsapp_bot",
  "qrcode": true,
  "number": "YOUR_PHONE_NUMBER",
  "integration": "WHATSAPP-BAILEYS"
}
```

### Step 3: Connect WhatsApp

1. Execute the workflow in n8n
2. Get QR code from response
3. Scan QR code with WhatsApp mobile app
4. Instance will be connected and ready
