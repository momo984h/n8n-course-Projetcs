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

## 🎯 Common Use Cases in n8n

### 1. **Send WhatsApp Messages**
```http
POST http://localhost:8080/message/sendText/my_whatsapp_bot
```

### 2. **Receive WhatsApp Messages**
- Set up webhook in Evolution API
- Configure n8n Webhook Trigger node
- Process incoming messages automatically

### 3. **Media Handling**
- Send images, documents, audio files
- Receive and process media from users
- Integrate with AI services for content analysis

## 🐛 Troubleshooting

### Common Issues

**❌ "Connection refused" error**
```bash
# Solution: Check if containers are running
docker ps
docker logs evolution_api
```

**❌ "Database connection failed"**
```bash
# Solution: Ensure PostgreSQL is ready
docker logs evolution_postgres
# Look for: "database system is ready to accept connections"
```

**❌ "QR Code not generating"**
```bash
# Solution: Check API logs and restart if needed
docker restart evolution_api
docker logs evolution_api
```

### Reset Everything

If you encounter persistent issues:

```bash
# Complete cleanup and restart
docker stop evolution_api evolution_postgres
docker rm -f evolution_api evolution_postgres
docker volume prune -f

# Then repeat Steps 2-4 from installation
```

## 🔄 Maintenance Commands

### Update Evolution API

```bash
# Stop current container
docker stop evolution_api
docker rm evolution_api

# Pull latest image
docker pull atendai/evolution-api:latest

# Restart with same configuration (repeat Step 4)
```

### Backup Database

```bash
# Create database backup
docker exec evolution_postgres pg_dump -U evolution evolution > evolution_backup.sql

# Restore from backup
docker exec -i evolution_postgres psql -U evolution evolution < evolution_backup.sql
```

## 📚 Advanced Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `AUTHENTICATION_API_KEY` | API authentication key | Required |
| `DATABASE_PROVIDER` | Database type | `postgresql` |
| `DATABASE_CONNECTION_URI` | Database connection string | Required |
| `SERVER_PORT` | API server port | `8080` |
| `WEBHOOK_URL` | Webhook endpoint for events | Optional |

### Production Deployment

For production environments:

1. **Use Docker Compose** for easier management
2. **Set up SSL/TLS** with reverse proxy (Nginx/Traefik)
3. **Configure proper backup** strategy
4. **Monitor logs** and performance
5. **Use secure API keys** and environment variables

## 🎥 Related Course Content

This installation guide supports the following course projects:

- **🔴 Advanced:** [Secured WhatsApp Multi-Agent System](../README.md#advanced-level-projects)
- **🔴 Advanced:** [WhatsApp Multi-Agent System](../README.md#advanced-level-projects)
- **🔴 Advanced:** [Multi-Modal WhatsApp Agent](../README.md#advanced-level-projects)

## 🆘 Getting Help

- **📺 YouTube Tutorial:** [Evolution API Installation Video](https://www.youtube.com/playlist?list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v)
- **📖 Official Docs:** [Evolution API Documentation](https://github.com/EvolutionAPI/evolution-api)
- **💬 Community:** Join our Discord for real-time support
- **🐛 Issues:** Report problems on [GitHub Issues](https://github.com/MohElshamy1994/n8n-course-Projetcs/issues)

## ⚠️ Important Notes

- 🔒 **Security:** Always change default passwords and API keys
- 📱 **WhatsApp Terms:** Ensure compliance with WhatsApp's Terms of Service
- 🚀 **Performance:** Monitor resource usage in production
- 🔄 **Updates:** Regularly update to the latest version for security patches

---

**🎯 Ready to integrate WhatsApp with your n8n workflows? Follow this guide step by step and you'll have a powerful messaging automation system up and running!**

**Happy Automating! 🤖✨**
