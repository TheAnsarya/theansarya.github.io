# Cloudflare Configuration Guide

Complete guide for Cloudflare settings used with ansarya.com and GitHub Pages.

## 📋 Overview

Cloudflare provides DNS management, CDN, SSL certificates, and security features for ansarya.com.

**Dashboard:** [dash.cloudflare.com](https://dash.cloudflare.com/)

---

## 🔧 Current Configuration

### Account Details
- **Domain:** ansarya.com
- **Plan:** Free
- **Status:** Active

### Quick Links
- [Cloudflare Dashboard](https://dash.cloudflare.com/)
- [DNS Settings](https://dash.cloudflare.com/?to=/:account/:zone/dns)
- [SSL/TLS Settings](https://dash.cloudflare.com/?to=/:account/:zone/ssl-tls)

---

## 🌐 DNS Configuration

### A Records (Root Domain)

| Type | Name | IPv4 Address | Proxy | TTL |
|------|------|--------------|-------|-----|
| A | @ | 185.199.108.153 | ✅ Proxied | Auto |
| A | @ | 185.199.109.153 | ✅ Proxied | Auto |
| A | @ | 185.199.110.153 | ✅ Proxied | Auto |
| A | @ | 185.199.111.153 | ✅ Proxied | Auto |

### CNAME Records

| Type | Name | Target | Proxy | TTL |
|------|------|--------|-------|-----|
| CNAME | www | theansarya.github.io | ✅ Proxied | Auto |

### How to Add DNS Records

1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Select your domain (ansarya.com)
3. Go to **DNS** → **Records**
4. Click **Add record**
5. Fill in the details:
   - **Type:** A or CNAME
   - **Name:** @ for root, www for subdomain
   - **IPv4/Target:** The IP or hostname
   - **Proxy status:** Enabled (orange cloud)
   - **TTL:** Auto
6. Click **Save**

---

## 🔒 SSL/TLS Settings

### Encryption Mode

**Location:** SSL/TLS → Overview

| Mode | Description | Recommended |
|------|-------------|-------------|
| Off | No encryption | ❌ Never |
| Flexible | HTTPS to Cloudflare, HTTP to origin | ⚠️ Not secure |
| **Full** | HTTPS end-to-end, any certificate | ✅ **Use this** |
| Full (strict) | HTTPS end-to-end, valid certificate | ✅ Best security |

**Current Setting:** Full

### Edge Certificates

**Location:** SSL/TLS → Edge Certificates

| Setting | Value | Description |
|---------|-------|-------------|
| Always Use HTTPS | ✅ On | Redirect all HTTP to HTTPS |
| Automatic HTTPS Rewrites | ✅ On | Fix mixed content |
| Minimum TLS Version | 1.2 | Modern security standard |
| TLS 1.3 | ✅ On | Latest TLS version |
| Opportunistic Encryption | ✅ On | Encrypt when possible |

### Certificate Status

Cloudflare provides a free Universal SSL certificate:
- **Type:** Universal (shared)
- **Coverage:** *.ansarya.com, ansarya.com
- **Validity:** Auto-renewing

---

## ⚡ Performance Settings

### Speed → Optimization

| Setting | Value | Description |
|---------|-------|-------------|
| Auto Minify (HTML) | ✅ On | Reduce HTML size |
| Auto Minify (CSS) | ✅ On | Reduce CSS size |
| Auto Minify (JS) | ✅ On | Reduce JavaScript size |
| Brotli | ✅ On | Better compression |
| Early Hints | ✅ On | Preload resources |
| Rocket Loader | ⚠️ Optional | Async JS loading |

### Caching

**Location:** Caching → Configuration

| Setting | Value |
|---------|-------|
| Caching Level | Standard |
| Browser Cache TTL | 4 hours |
| Always Online | ✅ On |

---

## 🛡️ Security Settings

### Security → Settings

| Setting | Value | Description |
|---------|-------|-------------|
| Security Level | Medium | Balance between security and accessibility |
| Challenge Passage | 30 minutes | How long a challenge is valid |
| Browser Integrity Check | ✅ On | Block bad bots |

### Firewall Rules (Optional)

For enhanced security, consider adding rules to:
- Block known bad IPs
- Rate limit requests
- Challenge suspicious traffic

---

## 📊 Analytics

### Overview

Cloudflare provides free analytics:
- **Requests:** Total and cached
- **Bandwidth:** Data transferred
- **Threats:** Blocked attacks
- **Countries:** Geographic distribution

**Location:** Analytics → Traffic

### Web Analytics

For detailed analytics, enable Cloudflare Web Analytics:
1. Go to **Analytics** → **Web Analytics**
2. Add your site
3. Copy the JavaScript snippet to your HTML

```html
<!-- Cloudflare Web Analytics -->
<script defer src='https://static.cloudflareinsights.com/beacon.min.js' 
        data-cf-beacon='{"token": "YOUR_TOKEN"}'></script>
```

---

## 🔧 Page Rules (Optional)

Create page rules for custom behavior:

### Force HTTPS
- **URL:** `http://*ansarya.com/*`
- **Setting:** Always Use HTTPS

### Cache Everything
- **URL:** `*ansarya.com/static/*`
- **Setting:** Cache Level: Cache Everything

### Bypass Cache for API
- **URL:** `*ansarya.com/api/*`
- **Setting:** Cache Level: Bypass

---

## 🚨 Troubleshooting

### "SSL Handshake Failed"

**Cause:** SSL mode mismatch

**Solution:**
1. Go to SSL/TLS → Overview
2. Change to **Full** (not Flexible or Strict)

### "Too Many Redirects"

**Cause:** Flexible SSL with HTTPS-enforcing origin

**Solution:**
1. Change SSL mode to **Full**
2. Wait 5 minutes
3. Clear browser cache

### "DNS Not Resolving"

**Cause:** Nameservers not updated or propagating

**Solution:**
1. Verify nameservers at registrar match Cloudflare's
2. Check propagation: [whatsmydns.net](https://www.whatsmydns.net/#NS/ansarya.com)
3. Wait up to 48 hours for full propagation

### "Orange Cloud Not Working"

**Cause:** Proxy disabled or unsupported record type

**Solution:**
1. Ensure proxy status shows orange cloud (not gray)
2. Verify record type supports proxying (A, AAAA, CNAME)

### "Certificate Not Issued"

**Cause:** Domain not fully activated

**Solution:**
1. Ensure domain is active in Cloudflare
2. Check SSL/TLS → Edge Certificates → Status
3. Wait up to 24 hours for Universal SSL

---

## 🔗 Integration with GitHub Pages

### How It Works

```
User → Cloudflare CDN → GitHub Pages Origin
         (HTTPS)           (HTTPS)
```

1. User requests `https://ansarya.com`
2. Cloudflare serves cached content or forwards to origin
3. GitHub Pages responds with content
4. Cloudflare caches and serves to user

### Benefits
- ✅ Global CDN for faster loading
- ✅ DDoS protection
- ✅ Free SSL certificate
- ✅ Analytics and insights
- ✅ Caching reduces GitHub Pages load

---

## 📚 Related Documentation

- [GitHub Pages Setup](github-pages-setup.md)
- [Custom Domain Setup](custom-domain-setup.md)
- [Marketplace Setup Guide](marketplace-setup-guide.md)

---

## 🔗 External Resources

- [Cloudflare DNS Documentation](https://developers.cloudflare.com/dns/)
- [Cloudflare SSL/TLS Modes](https://developers.cloudflare.com/ssl/origin-configuration/ssl-modes/)
- [Cloudflare with GitHub Pages](https://developers.cloudflare.com/support/third-party-software/others/configuring-cloudflare-and-github/)
- [Troubleshooting SSL](https://developers.cloudflare.com/ssl/troubleshooting/)

---

*Last Updated: January 16, 2026*
