# 🌐 Hostinger Domain Setup Guide for n8n

This comprehensive guide will walk you through configuring a custom domain name with your n8n installation on Hostinger VPS. By the end of this guide, you'll have your n8n instance accessible via your custom domain with SSL encryption.

## 📋 Prerequisites

Before starting, ensure you have:
- ✅ A Hostinger VPS with n8n already installed via Docker
- ✅ A custom domain name (e.g., `elshamyn8n.shop`)
- ✅ Access to Hostinger Control Panel
- ✅ Your VPS IP address (e.g., `212.1.213.181`)

## 🚀 Step-by-Step Setup Process

### Step 1: Verify Domain Resolution

Before making any changes, it's crucial to verify that your domain is properly pointing to your VPS.

#### Option A: Online DNS Checker
1. Visit: [https://dnschecker.org/#A/your-domain.com](https://dnschecker.org/#A/elshamyn8n.shop)
2. Replace `your-domain.com` with your actual domain name
3. Verify the A record shows your VPS IP address: `212.1.213.181`
4. Wait until you see green checkmarks globally (can take up to 24 hours)

#### Option B: Command Line Check
Open terminal/command prompt and run:
```bash
nslookup elshamyn8n.shop
```
**Expected output:**
```
Name:    elshamyn8n.shop
Address: 212.1.213.181
```

> **⏰ Note**: DNS propagation can take up to 24 hours. Don't proceed until your domain resolves correctly.

---

### Step 2: Configure DNS in Hostinger Control Panel

1. **Access Hostinger Control Panel**
   - Log into your Hostinger account
   - Navigate to **DNS Manager** or **Domain Management**

2. **Add/Verify DNS Records**
   Configure the following DNS records:

   | Type | Name | Value | TTL |
   |------|------|-------|-----|
   | **A** | `@` (or leave blank for root domain) | `212.1.213.181` | `14400` (4 hours) |
   | **A** | `www` | `212.1.213.181` | `14400` (4 hours) |

3. **Save Changes**
   - Click **Save** or **Update Records**
   - Allow 15-30 minutes for changes to take effect

> **💡 Pro Tip**: The `@` symbol represents your root domain. Some DNS managers require you to leave the name field blank instead.

---

### Step 3: Update Environment File

Create or update your `.env` file in your n8n Docker project directory:

```bash
# Navigate to your n8n project directory
cd /path/to/your/n8n-project

# Create or edit the .env file
nano .env
```

**Add the following configuration:**
```env
# Domain Configuration
DOMAIN_NAME=elshamyn8n.shop
SUBDOMAIN=
GENERIC_TIMEZONE=America/Denver
SSL_EMAIL=user@srv1006627.hstgr.cloud

# Additional recommended settings
N8N_PROTOCOL=https
N8N_PORT=443
WEBHOOK_URL=https://elshamyn8n.shop/
```

> **📧 SSL Email**: Replace `user@srv1006627.hstgr.cloud` with your actual email address for SSL certificate notifications.

---

### Step 4: Update Docker Compose Configuration

Your `docker-compose.yml` file needs specific modifications to work with a root domain instead of a subdomain.

#### Find and Replace These Lines:

**❌ FROM (subdomain configuration):**
```yaml
- traefik.http.routers.n8n.rule=Host(`${SUBDOMAIN}.${DOMAIN_NAME}`)
- N8N_HOST=${SUBDOMAIN}.${DOMAIN_NAME}
- WEBHOOK_URL=https://${SUBDOMAIN}.${DOMAIN_NAME}/
```

**✅ TO (root domain configuration):**
```yaml
- traefik.http.routers.n8n.rule=Host(`${DOMAIN_NAME}`)
- N8N_HOST=${DOMAIN_NAME}
- WEBHOOK_URL=https://${DOMAIN_NAME}/
```

#### Complete Docker Compose Example:

```yaml
version: '3.8'

services:
  traefik:
    image: traefik:v2.10
    container_name: traefik
    restart: unless-stopped
    command:
      - --api.dashboard=true
      - --api.insecure=false
      - --providers.docker=true
      - --providers.docker.exposedbydefault=false
      - --entrypoints.web.address=:80
      - --entrypoints.websecure.address=:443
      - --certificatesresolvers.myresolver.acme.email=${SSL_EMAIL}
      - --certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json
      - --certificatesresolvers.myresolver.acme.tlschallenge=true
      - --entrypoints.web.http.redirections.entrypoint.to=websecure
      - --entrypoints.web.http.redirections.entrypoint.scheme=https
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - traefik_letsencrypt:/letsencrypt
    labels:
      - traefik.enable=true
      - traefik.http.routers.traefik.rule=Host(`traefik.${DOMAIN_NAME}`)
      - traefik.http.routers.traefik.entrypoints=websecure
      - traefik.http.routers.traefik.tls.certresolver=myresolver
      - traefik.http.routers.traefik.service=api@internal

  n8n:
    image: docker.n8n.io/n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=${POSTGRES_DB}
      - DB_POSTGRESDB_USER=${POSTGRES_USER}
      - DB_POSTGRESDB_PASSWORD=${POSTGRES_PASSWORD}
      - N8N_HOST=${DOMAIN_NAME}
      - N8N_PORT=443
      - N8N_PROTOCOL=https
      - NODE_ENV=production
      - WEBHOOK_URL=https://${DOMAIN_NAME}/
      - N8N_DEFAULT_BINARY_DATA_MODE=filesystem
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
    volumes:
      - n8n_data:/home/node/.n8n
    labels:
      - traefik.enable=true
      - traefik.http.routers.n8n.rule=Host(`${DOMAIN_NAME}`)
      - traefik.http.routers.n8n.entrypoints=websecure
      - traefik.http.routers.n8n.tls.certresolver=myresolver
    depends_on:
      - postgres
      - traefik

  postgres:
    image: postgres:13
    container_name: postgres
    restart: unless-stopped
    environment:
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  n8n_data:
  postgres_data:
  traefik_letsencrypt:
```

---

### Step 5: Deploy/Restart Containers

#### Option A: Hostinger Docker Manager (Recommended)
1. **Access Docker Manager**
   - Go to Hostinger Control Panel
   - Navigate to **VPS** → **Docker Manager**

2. **Update Your Project**
   - Click **"Edit"** on your n8n project
   - Upload your updated `docker-compose.yml` and `.env` files
   - Look for **"Restart"** or **"Redeploy"** option
   - Restart both containers (traefik and n8n)

#### Option B: Command Line
If you have SSH access to your VPS:

```bash
# Navigate to your project directory
cd /path/to/your/n8n-project

# Stop existing containers
docker-compose down

# Pull latest images (optional)
docker-compose pull

# Start containers with new configuration
docker-compose up -d

# Check container status
docker-compose ps
```

---

## ✅ Verification Steps

After completing the setup, verify everything is working correctly:

### 1. **Check Container Status**
```bash
docker-compose ps
```
All containers should show "Up" status.

### 2. **Test Domain Access**
1. Open your browser
2. Navigate to: `https://yourdomain.com` (e.g., `https://elshamyn8n.shop`)
3. You should see the n8n login page with a valid SSL certificate (green lock icon)

### 3. **Verify SSL Certificate**
- Check for the green lock icon in your browser
- Click on the lock to view certificate details
- Certificate should be issued by "Let's Encrypt Authority"

### 4. **Test Webhooks**
Create a test webhook in n8n and verify it works with your new domain.

---

## 🚨 Troubleshooting

### Common Issues and Solutions

#### **Issue: "Site can't be reached" or timeout errors**
**Solution:**
- Verify DNS propagation is complete (use Step 1)
- Check if ports 80 and 443 are open on your VPS firewall
- Ensure Docker containers are running: `docker-compose ps`

#### **Issue: "SSL certificate error" or "Not secure" warning**
**Solution:**
- Wait 5-10 minutes for Let's Encrypt to issue certificate
- Check traefik logs: `docker-compose logs traefik`
- Verify your email in SSL_EMAIL is valid
- Restart traefik container: `docker-compose restart traefik`

#### **Issue: "404 Not Found" when accessing the domain**
**Solution:**
- Check n8n container logs: `docker-compose logs n8n`
- Verify `N8N_HOST` environment variable matches your domain exactly
- Ensure traefik labels are correctly configured

#### **Issue: Old subdomain still accessible**
**Solution:**
- Clear browser cache and cookies
- Use incognito/private browsing mode
- Check if old containers are still running: `docker ps -a`

### **Debugging Commands**

```bash
# Check all running containers
docker ps

# View container logs
docker-compose logs [service-name]

# Check Docker Compose configuration
docker-compose config

# Restart specific service
docker-compose restart [service-name]

# View traefik dashboard (if enabled)
# Access: https://traefik.yourdomain.com
```

---

## 🔒 Security Best Practices

### 1. **Firewall Configuration**
Ensure your VPS firewall only allows necessary ports:
- **Port 22**: SSH (restrict to your IP if possible)
- **Port 80**: HTTP (for Let's Encrypt challenges)
- **Port 443**: HTTPS (for n8n access)

### 2. **Regular Updates**
Keep your system and Docker images updated:
```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Update Docker images
docker-compose pull
docker-compose up -d
```

### 3. **Backup Strategy**
Regular backup your n8n data and configuration:
```bash
# Backup n8n data
docker-compose exec n8n tar -czf /tmp/n8n-backup.tar.gz /home/node/.n8n
docker cp n8n:/tmp/n8n-backup.tar.gz ./n8n-backup-$(date +%Y%m%d).tar.gz
```

---

## 📞 Support

If you encounter issues not covered in this guide:

1. **Check the main course** for additional troubleshooting videos
2. **Review Docker logs** using the commands provided above
3. **Verify DNS settings** using online tools
4. **Ask in the course community** with specific error messages

---

## 📚 Additional Resources

- [Traefik Documentation](https://doc.traefik.io/traefik/)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
- [n8n Self-Hosting Guide](https://docs.n8n.io/hosting/)
- [Docker Compose Reference](https://docs.docker.com/compose/)

---

**🎉 Congratulations!** Your n8n instance should now be accessible via your custom domain with SSL encryption. You can now create workflows with webhook URLs using your professional domain name.

**⭐ Next Steps**: Start building your AI automation workflows with confidence, knowing your setup is production-ready and secure!
