# Evolution API - Setup Guide for Students

## 📚 Table of Contents
- [Prerequisites](#prerequisites)
- [Step 1: Clone the Repository](#step-1-clone-the-repository)
- [Step 2: Configure Environment Variables](#step-2-configure-environment-variables)
- [Step 3: Update Docker Compose Configuration](#step-3-update-docker-compose-configuration)
- [Step 4: Start the Application](#step-4-start-the-application)
- [Step 5: Verify Installation](#step-5-verify-installation)
- [Common Commands](#common-commands)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

---

## Prerequisites

Before starting, make sure you have the following installed on your computer:

1. **Git** - Download from [git-scm.com](https://git-scm.com/downloads)
2. **Docker Desktop** - Download from [docker.com](https://www.docker.com/products/docker-desktop/)
   - For Windows: Requires Windows 10/11 Pro, Enterprise, or Education
   - For Mac: Requires macOS 10.15 or newer
   - For Linux: Install Docker Engine and Docker Compose

### Verify Prerequisites

Open your terminal/command prompt and run:

```bash
# Check Git installation
git --version

# Check Docker installation
docker --version

# Check Docker Compose installation
docker-compose --version
```

If all commands return version numbers, you're ready to proceed!

---

## Step 1: Clone the Repository

### Option A: Using HTTPS (Recommended for beginners)

Open your terminal and navigate to where you want to store the project:

```bash
# Navigate to your desired folder (example)
cd ~/Documents

# Clone the repository
git clone https://github.com/EvolutionAPI/evolution-api.git

# Enter the project directory
cd evolution-api
```

### Option B: Using SSH (For advanced users with SSH keys configured)

```bash
git clone git@github.com:EvolutionAPI/evolution-api.git
cd evolution-api
```

---

## Step 2: Configure Environment Variables

### 2.1 Create the `.env` file

The `.env` file contains all the configuration for your Evolution API instance.

**Windows (PowerShell):**
```powershell
Copy-Item env.example .env
```

**macOS/Linux:**
```bash
cp env.example .env
```

### 2.2 Edit the `.env` file

Open the `.env` file with your favorite text editor (VS Code, Notepad++, etc.) and update these **critical settings**:

#### Required Changes:

```ini
# ===========================================
# DATABASE CONFIGURATION
# ===========================================
DATABASE_PROVIDER=postgresql
DATABASE_CONNECTION_URI=postgresql://postgres:evolution_password@evolution-postgres:5432/evolution_db

# Add these PostgreSQL variables (REQUIRED!)
POSTGRES_DATABASE=evolution_db
POSTGRES_USERNAME=postgres
POSTGRES_PASSWORD=evolution_password

# ===========================================
# REDIS CONFIGURATION
# ===========================================
CACHE_REDIS_ENABLED=true
CACHE_REDIS_URI=redis://evolution-redis:6379

# ===========================================
# AUTHENTICATION (⚠️ CHANGE THIS!)
# ===========================================
AUTHENTICATION_API_KEY=YOUR_SUPER_SECRET_API_KEY_HERE

# ===========================================
# SERVER CONFIGURATION
# ===========================================
SERVER_URL=http://localhost:8080
```

#### ⚠️ Important Notes:

1. **Database credentials**: Use `evolution-postgres` as the hostname (not `localhost`) because Docker containers communicate using service names
2. **Redis URI**: Use `evolution-redis` as the hostname
3. **API Key**: Change `YOUR_SUPER_SECRET_API_KEY_HERE` to a strong, random string
   - Example: `MySecureKey2024!@#$`
   - Or generate one online: [randomkeygen.com](https://randomkeygen.com/)

#### Optional Changes:

```ini
# Language (default is Portuguese)
LANGUAGE=en

# Enable/disable telemetry
TELEMETRY_ENABLED=true

# Webhook events (set to true for events you need)
WEBHOOK_EVENTS_MESSAGES_UPSERT=true
WEBHOOK_EVENTS_CONNECTION_UPDATE=true
```

---

## Step 3: Update Docker Compose Configuration

The default `docker-compose.yaml` may have external network configurations that need to be removed.

### 3.1 Open `docker-compose.yaml`

Open the `docker-compose.yaml` file in your text editor.

### 3.2 Check Network Configuration

Find the `networks:` section at the **bottom** of the file. It should look like this:

#### ✅ Correct Configuration:

```yaml
networks:
  evolution-net:
    name: evolution-net
    driver: bridge
```

#### ❌ If you see this (INCORRECT):

```yaml
networks:
  evolution-net:
    name: evolution-net
    driver: bridge
  dokploy-network:
    external: true
```

**Remove** the `dokploy-network` section entirely.

### 3.3 Update Port Binding (Optional)

If you want the API accessible from other computers on your network:

Find this line:
```yaml
ports:
  - "127.0.0.1:8080:8080"
```

Change it to:
```yaml
ports:
  - "8080:8080"
```

### 3.4 Final `docker-compose.yaml` Structure

Your file should look like this:

```yaml
version: "3.8"

services:
  api:
    container_name: evolution_api
    image: evoapicloud/evolution-api:latest
    restart: always
    depends_on:
      - redis
      - evolution-postgres
    ports:
      - "8080:8080"
    volumes:
      - evolution_instances:/evolution/instances
    networks:
      - evolution-net
    env_file:
      - .env
    expose:
      - "8080"

  redis:
    container_name: evolution_redis
    image: redis:latest
    restart: always
    command: >
      redis-server --port 6379 --appendonly yes
    volumes:
      - evolution_redis:/data
    networks:
      evolution-net:
        aliases:
          - evolution-redis
    expose:
      - "6379"

  evolution-postgres:
    container_name: evolution_postgres
    image: postgres:15
    restart: always
    env_file:
      - .env
    command:
      - postgres
      - -c
      - max_connections=1000
      - -c
      - listen_addresses=*
    environment:
      - POSTGRES_DB=${POSTGRES_DATABASE}
      - POSTGRES_USER=${POSTGRES_USERNAME}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - evolution-net
    expose:
      - "5432"

volumes:
  evolution_instances:
  evolution_redis:
  postgres_data:

networks:
  evolution-net:
    name: evolution-net
    driver: bridge
```

---

## Step 4: Start the Application

### 4.1 Start Docker Desktop

Make sure **Docker Desktop** is running on your computer.

### 4.2 Start Evolution API

Open your terminal in the `evolution-api` directory and run:

```bash
# Start all services in detached mode (background)
docker-compose up -d
```

### 4.3 Wait for Initialization

The first time you run this, Docker will:
1. Download required images (~500MB-1GB)
2. Create volumes for data storage
3. Start containers
4. Run database migrations
5. Initialize the API

This may take **3-5 minutes** on the first run.

### 4.4 Monitor the Startup

Watch the logs to see when the API is ready:

```bash
# View logs from all services
docker-compose logs -f

# Or view only API logs
docker-compose logs -f api
```

**Look for this message** indicating the server is ready:

```
[SERVER] HTTP - ON: 8080
```

Press `Ctrl+C` to stop viewing logs (this won't stop the containers).

---

## Step 5: Verify Installation

### 5.1 Check Container Status

```bash
docker-compose ps
```

You should see 3 containers running:
- `evolution_api` - The main API
- `evolution_postgres` - PostgreSQL database
- `evolution_redis` - Redis cache

### 5.2 Test API Connection

Open your browser and visit:

```
http://localhost:8080/manager
```

Or test with curl:

```bash
curl -X GET http://localhost:8080 -H "apikey: YOUR_API_KEY_FROM_ENV_FILE"
```

### 5.3 Access Points

| Service | URL | Purpose |
|---------|-----|---------|
| **Manager UI** | http://localhost:8080/manager | Web interface for managing WhatsApp instances |
| **API Documentation** | http://localhost:8080/docs | Swagger/OpenAPI documentation |
| **API Endpoint** | http://localhost:8080 | REST API endpoint |

---

## Common Commands

### View Logs

```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f api
docker-compose logs -f evolution-postgres
docker-compose logs -f redis

# Last 50 lines only
docker-compose logs --tail 50 api
```

### Stop Services

```bash
# Stop all services (data is preserved)
docker-compose down

# Stop and remove all data (⚠️ destructive!)
docker-compose down -v
```

### Restart Services

```bash
# Restart all services
docker-compose restart

# Restart specific service
docker-compose restart api
```

### Update to Latest Version

```bash
# Pull latest images
docker-compose pull

# Restart with new images
docker-compose up -d
```

### Access Container Shell

```bash
# Access API container bash
docker exec -it evolution_api bash

# Access PostgreSQL
docker exec -it evolution_postgres psql -U postgres -d evolution_db
```

---

## Troubleshooting

### Problem: Port 8080 is already in use

**Solution:** Change the port in `docker-compose.yaml`:

```yaml
ports:
  - "8081:8080"  # Changed from 8080 to 8081
```

Then access the API at `http://localhost:8081`

### Problem: Database connection failed

**Check:**
1. Is PostgreSQL container running? `docker-compose ps`
2. Are the environment variables correct in `.env`?
3. Check PostgreSQL logs: `docker-compose logs evolution-postgres`

**Solution:**
```bash
# Restart PostgreSQL
docker-compose restart evolution-postgres

# Or recreate all containers
docker-compose down
docker-compose up -d
```

### Problem: Redis connection failed

**Check:**
1. Is Redis container running? `docker-compose ps`
2. Is `CACHE_REDIS_URI` set to `redis://evolution-redis:6379`?

**Solution:**
```bash
docker-compose restart redis
```

### Problem: Cannot access http://localhost:8080

**Check:**
1. Is the API container running? `docker-compose ps`
2. Check firewall settings
3. Try accessing from the same machine first

**View API logs:**
```bash
docker-compose logs api
```

### Problem: "Error: Cannot find module..."

**Solution:**
```bash
# Rebuild the API container
docker-compose up -d --build api
```

### Problem: Database migrations failed

**Solution:**
```bash
# Stop everything
docker-compose down -v

# Start fresh
docker-compose up -d
```

---

## Next Steps

### 1. Create Your First WhatsApp Instance

Visit the Manager UI at http://localhost:8080/manager and:
1. Click "Create Instance"
2. Enter an instance name
3. Scan the QR code with your WhatsApp mobile app
4. Start sending messages via API!

### 2. Read the API Documentation

- **Official Docs:** https://doc.evolution-api.com
- **Swagger UI:** http://localhost:8080/docs
- **Postman Collection:** https://evolution-api.com/postman

### 3. Test API Endpoints

Example: Send a text message

```bash
curl -X POST http://localhost:8080/message/sendText/YOUR_INSTANCE_NAME \
  -H "apikey: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "number": "5511999999999",
    "text": "Hello from Evolution API!"
  }'
```

### 4. Explore Integrations

Evolution API supports integrations with:
- **Typebot** - Conversational chatbots
- **Chatwoot** - Customer support
- **OpenAI** - AI-powered responses
- **Dify** - AI workflows
- **Webhooks** - Real-time event notifications
- **RabbitMQ / SQS** - Message queuing

Enable them in your `.env` file!

---

## Security Best Practices

1. ✅ **Change the default API key** to a strong random string
2. ✅ **Use environment variables** - never hardcode credentials
3. ✅ **Enable HTTPS** in production (use reverse proxy like Nginx)
4. ✅ **Restrict network access** - use firewall rules
5. ✅ **Regular backups** - backup Docker volumes regularly
6. ✅ **Keep updated** - regularly update to latest version

---

## Additional Resources

- **GitHub Repository:** https://github.com/EvolutionAPI/evolution-api
- **Official Documentation:** https://doc.evolution-api.com
- **Discord Community:** https://evolution-api.com/discord
- **WhatsApp Group:** https://evolution-api.com/whatsapp
- **Report Issues:** https://github.com/EvolutionAPI/evolution-api/issues

---

## Support

If you encounter any issues:

1. Check the [Troubleshooting](#troubleshooting) section
2. Review logs: `docker-compose logs -f`
3. Search existing [GitHub Issues](https://github.com/EvolutionAPI/evolution-api/issues)
4. Join the [Discord Community](https://evolution-api.com/discord)
5. Ask your instructor for help

---

## License

Evolution API is licensed under Apache License 2.0.  
© 2025 Evolution API

---

**Happy Coding! 🚀**

