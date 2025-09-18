# Evolution API Installation with Hostinger VPS

Complete guide for installing and configuring Evolution API on Hostinger VPS with custom subdomain setup.

## Prerequisites
- Hostinger VPS account
- Domain already configured with Hostinger

**📺 Video Tutorial**: [🎥 Complete Setup Guide](https://www.youtube.com/watch?v=eIyUlX0o-kQ&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=67&t=1s)

## Step 1: DNS Subdomain Configuration

### Setting up the Evolution API Subdomain

We need to create a subdomain for the Evolution API. In this guide, we'll use `evo` as the subdomain.

1. **Access Hostinger DNS Management**
   - Log into your Hostinger account
   - Navigate to your domain management panel
   - Go to DNS / Nameservers section

2. **Add DNS Record for Evolution API**
   - We'll create a subdomain called `evo` for our Evolution API
   - The new DNS will be: `https://evo.elshamyn8n.shop/`

3. **DNS Configuration**
   - Add a new A record:
     - **Type**: A
     - **Name**: evo
     - **Points to**: Your VPS IP address (212.213.181)
     - **TTL**: 3600

![DNS Configuration](images/hostinger-dns-evo-subdomain.png)

*Screenshot showing the Hostinger DNS management interface with the `evo` subdomain A record pointing to IP 212.213.181 with TTL 300*

## Step 2: Verify DNS Connection

After configuring the DNS record, verify that the subdomain is properly pointing to your VPS IP address.

### Using dig command

Run the following command to check the DNS resolution:

```bash
dig +short evo.elshamyn8n.shop
```

**Expected Output:**
```
212.1.213.181
```

This confirms that the `evo` subdomain is correctly resolving to your VPS IP address.

## Step 3: Deploy Evolution API with Docker

### 3.1 Create Project Directory

In your VPS terminal, create a new folder for the Evolution API:

```bash
mkdir -p ~/evolution-api && cd ~/evolution-api
```

### 3.2 Create Environment File

Create the `.env` file with the required configuration:

```bash
cat > .env <<'EOF'
AUTHENTICATION_API_KEY=put your API Key
SERVER_TYPE=http
SERVER_PORT=8080
CONFIG_SESSION_PHONE_VERSION=2.3000.1023204200
SERVER_URL=https://evo.elshamyn8n.shop
DATABASE_ENABLED=true
DATABASE_PROVIDER=postgresql
DATABASE_CONNECTION_URI=postgresql://postgres:postgrespass@postgres:5432/evolution?schema=public
CACHE_REDIS_ENABLED=true
CACHE_REDIS_URI=redis://redis:6379/1
CACHE_REDIS_PREFIX_KEY=evolution_v2
EOF
```

### 3.3 Create Docker Compose File

Create the `docker-compose.yml` file:

```bash
cat > docker-compose.yml <<'EOF'
version: '3.9'

services:
  evolution-api:
    image: atendai/evolution-api:v2.1.1
    container_name: evolution_api
    restart: always
    env_file: [.env]
    volumes:
      - evolution_instances:/evolution/instances
    networks: [traefik_net]
    labels:
      - traefik.enable=true
      - traefik.http.routers.evo.rule=Host(`evo.elshamyn8n.shop`)
      - traefik.http.routers.evo.entrypoints=web,websecure     # <<< SAME as n8n
      - traefik.http.routers.evo.tls=true
      - traefik.http.routers.evo.tls.certresolver=mytlschallenge # <<< SAME as n8n
      - traefik.http.services.evo.loadbalancer.server.port=8080
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15
    container_name: evolution_postgres
    restart: always
    environment:
      POSTGRES_DB: evolution
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgrespass
    volumes:
      - evolution_pg:/var/lib/postgresql/data
    networks: [traefik_net]

  redis:
    image: redis:7-alpine
    container_name: evolution_redis
    restart: always
    command: ["redis-server","--appendonly","yes"]
    networks: [traefik_net]

volumes:
  evolution_instances:
  evolution_pg:

networks:
  traefik_net:
    external: true
    name: root_default   # the network where your traefik+n8n stack runs
EOF
```

### 3.4 Deploy the Compose

Deploy the Evolution API stack:

```bash
docker compose up -d
```

---

*Next steps will be added in the following sections...*
