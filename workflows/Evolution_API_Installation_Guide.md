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

## 🚀 Evolution API Installation - Latest Version (Final Steps)

### Step 1: Install WSL2 (Windows Only)

```cmd
wsl --install
```
Restart computer when prompted.

### Step 2: Clone Evolution API Repository

```cmd
git clone https://github.com/EvolutionAPI/evolution-api.git
cd evolution-api
```

### Step 3: Setup Environment Configuration

```cmd
cp .env.example .env
```

**Edit the `.env` file and change the API Key:**
- Open `.env` file in any text editor
- Find: `AUTHENTICATION_API_KEY=`
- Change to: `AUTHENTICATION_API_KEY=EvolutionAPI_MySecure_Key_2024_789xyz`

### Step 4: Replace Docker Compose File

Replace the existing `docker-compose.yaml` with the provided configuration file.

**[📁 Download docker-compose.yaml](../docker-compose.yaml)**

### Step 5: Start All Services

```cmd
docker compose up -d
```

### Step 6: Verify Installation

```cmd
# Check all containers are running
docker compose ps

# Check Evolution API logs
docker compose logs evolution_api

# Test API endpoint
curl http://localhost:8080
```

**Expected: 3 containers running**
- `evolution_api`
- `postgres`
- `redis`

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

## 📥 **Download Complete Installation Package**

For easier installation, download the complete package with all required files:

**[📦 Download Evolution_API_Installation.zip](Evolution_API_Installation.zip)**

This ZIP file contains:
- `docker-compose.yaml` - Complete Docker configuration
- `.env.example` - Environment template file  
- `README.md` - Quick setup instructions

**Installation:** Extract the ZIP file and follow the steps above.

