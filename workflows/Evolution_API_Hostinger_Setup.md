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

### Verification

After setting up the DNS record, you can verify the Evolution API is working by visiting:
```
https://evo.elshamyn8n.shop/
```

You should see a response like:
```json
{
  "status": 200,
  "message": "Welcome to the Evolution API, it is working!",
  "version": "2.1.1",
  "clientName": "evolution_exchange",
  "manager": "http://evo.elshamyn8n.shop/manager",
  "documentation": "https://doc.evolution-api.com"
}
```

---

*Next steps will be added in the following sections...*
