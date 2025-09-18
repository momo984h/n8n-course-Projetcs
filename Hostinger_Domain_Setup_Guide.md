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
version: "3.7"

services:
  traefik:
    image: "traefik"
    restart: always
    command:
      - "--api=true"
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.web.http.redirections.entryPoint.to=websecure"
      - "--entrypoints.web.http.redirections.entrypoint.scheme=https"
      - "--entrypoints.websecure.address=:443"
      - "--certificatesresolvers.mytlschallenge.acme.tlschallenge=true"
      - "--certificatesresolvers.mytlschallenge.acme.email=${SSL_EMAIL}"
      - "--certificatesresolvers.mytlschallenge.acme.storage=/letsencrypt/acme.json"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - traefik_data:/letsencrypt
      - /var/run/docker.sock:/var/run/docker.sock:ro

  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    ports:
      - "127.0.0.1:5678:5678"
    labels:
      - traefik.enable=true
      - traefik.http.routers.n8n.rule=Host(${DOMAIN_NAME})
      - traefik.http.routers.n8n.tls=true
      - traefik.http.routers.n8n.entrypoints=web,websecure
      - traefik.http.routers.n8n.tls.certresolver=mytlschallenge
      - traefik.http.middlewares.n8n.headers.SSLRedirect=true
      - traefik.http.middlewares.n8n.headers.STSSeconds=315360000
      - traefik.http.middlewares.n8n.headers.browserXSSFilter=true
      - traefik.http.middlewares.n8n.headers.contentTypeNosniff=true
      - traefik.http.middlewares.n8n.headers.forceSTSHeader=true
      - traefik.http.middlewares.n8n.headers.SSLHost=${DOMAIN_NAME}
      - traefik.http.middlewares.n8n.headers.STSIncludeSubdomains=true
      - traefik.http.middlewares.n8n.headers.STSPreload=true
      - traefik.http.routers.n8n.middlewares=n8n@docker
    environment:
      - N8N_HOST=${DOMAIN_NAME}
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - NODE_ENV=production
      - WEBHOOK_URL=https://${DOMAIN_NAME}/
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
    volumes:
      - n8n_data:/home/node/.n8n
      - /local-files:/files

volumes:
  traefik_data:
    external: true
  n8n_data:
    external: true
```

---

### Step 5: Deploy/Restart Containers

Go to Hostinger Control Panel → VPS → Docker Manager, click "Edit" on your n8n project, upload your updated files, and restart both containers (traefik and n8n).

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


---

## 📚 Additional Resources

- [Traefik Documentation](https://doc.traefik.io/traefik/)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
- [n8n Self-Hosting Guide](https://docs.n8n.io/hosting/)
- [Docker Compose Reference](https://docs.docker.com/compose/)

---

**🎉 Congratulations!** Your n8n instance should now be accessible via your custom domain with SSL encryption. You can now create workflows with webhook URLs using your professional domain name.

**⭐ Next Steps**: Start building your AI automation workflows with confidence, knowing your setup is production-ready and secure!
