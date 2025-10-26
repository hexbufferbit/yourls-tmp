# Production Setup - Behind CDN/Load Balancer

## Architecture

```
Users → CDN/CloudFlare (HTTPS:443) → Your Server (HTTP:9001) → YOURLS
```

Your CDN handles:
- ✅ SSL/TLS termination
- ✅ DDoS protection  
- ✅ Global caching
- ✅ Compression

Your server handles:
- ✅ YOURLS application logic
- ✅ Database operations
- ✅ URL shortening/redirects

## Deployment Steps

1. **Copy production environment:**
   ```bash
   cp .env.production .env
   # Edit .env with your actual domain and strong passwords
   ```

2. **Deploy with production optimizations:**
   ```bash
   docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build
   ```

3. **Configure your CDN:**
   - Point your domain to your server's IP
   - Set CDN to forward to port 9001
   - Enable "Full" SSL mode (CDN to origin)
   - Forward headers: `X-Forwarded-Proto: https`

4. **Test your setup:**
   - Visit `https://yourdomain.com/admin` (via CDN)
   - Create a test short URL
   - Verify the short URL works

## What's Different in Production?

The same Docker image works for both local and production with these automatic optimizations:

**Local Development:**
- More verbose Apache logging
- Development-friendly server signatures

**Production (via docker-compose.prod.yml):**
- `ServerTokens Prod` (hide Apache version)
- `ServerSignature Off` (hide server info)
- Warning-level logging only
- CDN headers automatically detected

## CDN Configuration Examples

### CloudFlare
- **SSL/TLS Mode**: Full (not Full Strict)
- **Always Use HTTPS**: ON
- **Origin Port**: 9001

### AWS CloudFront
- **Origin Protocol**: HTTP Only
- **Origin Port**: 9001
- **Forward Headers**: `X-Forwarded-Proto`

### Other CDNs
- Point to `your-server-ip:9001`
- Forward `X-Forwarded-Proto: https` header
- Use HTTP (not HTTPS) to origin

## Benefits

- 🚀 **Simple**: One Dockerfile for all environments
- 🔒 **Secure**: CDN handles SSL and DDoS
- 💰 **Cost-effective**: No SSL certificates on server
- 🔧 **Easy debugging**: HTTP backend
- 📊 **Insights**: CDN analytics included