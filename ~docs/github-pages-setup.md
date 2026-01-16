# GitHub Pages Setup Guide

Complete guide for setting up GitHub Pages for the TheAnsarya organization.

## 📋 Overview

GitHub Pages is a static site hosting service that takes files from a repository and publishes them as a website.

**Current Site:** [ansarya.com](https://ansarya.com) / [theansarya.github.io](https://theansarya.github.io)

---

## 🔧 Configuration Summary

### Repository Settings
- **Repository:** [TheAnsarya/theansarya.github.io](https://github.com/TheAnsarya/theansarya.github.io)
- **Branch:** `main`
- **Source Folder:** `/ (root)`
- **Custom Domain:** `ansarya.com`
- **HTTPS Enforced:** ✅ Yes

### Quick Links
- [Repository Settings](https://github.com/TheAnsarya/theansarya.github.io/settings)
- [Pages Settings](https://github.com/TheAnsarya/theansarya.github.io/settings/pages)
- [Actions (Deployments)](https://github.com/TheAnsarya/theansarya.github.io/actions)

---

## 📝 Step-by-Step Setup

### Step 1: Create Repository

For user/organization sites:
- Repository name **must** be `<username>.github.io`
- Example: `TheAnsarya/theansarya.github.io`

For project sites:
- Any repository name works
- URL will be `<username>.github.io/<repository>`

### Step 2: Enable GitHub Pages

1. Navigate to your repository on GitHub
2. Click **Settings** (gear icon in top bar)
3. In left sidebar, click **Pages** (under "Code and automation")
4. Under **Build and deployment**:
	- **Source:** Deploy from a branch
	- **Branch:** `main` (or your default branch)
	- **Folder:** `/ (root)` or `/docs`
5. Click **Save**

### Step 3: Add Content

Minimum required file structure:
```
/
├── index.html			# Main page
├── CNAME				# Custom domain (optional)
├── README.md			# Repository description
└── pages/				# Additional pages
	├── repositories.md
	└── plans.md
```

### Step 4: Verify Deployment

1. Go to **Settings → Pages**
2. You should see: "Your site is live at https://..."
3. Click the link to visit your site
4. Check **Actions** tab for deployment status

---

## 🌐 Custom Domain Setup

### Add CNAME File

Create a `CNAME` file in your repository root:
```
ansarya.com
```

**Important:**
- No `https://` prefix
- No `www.` prefix (unless you specifically want www as primary)
- Just the bare domain

### Configure in GitHub

1. Go to **Settings → Pages**
2. Under **Custom domain**, enter: `ansarya.com`
3. Click **Save**
4. GitHub performs a DNS check
5. Once verified, check **Enforce HTTPS**

### DNS Requirements

GitHub Pages requires these A records pointing to:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

And a CNAME for www:
```
www → theansarya.github.io
```

---

## 🔒 HTTPS Configuration

### GitHub-managed HTTPS
- Automatically provisions Let's Encrypt certificate
- Works with custom domains
- May take up to 24 hours after DNS configuration

### Cloudflare-managed HTTPS
- Faster certificate provisioning
- Additional security features
- See [cloudflare-settings.md](cloudflare-settings.md) for details

### Enabling HTTPS

1. Ensure DNS is properly configured
2. Wait for DNS propagation
3. Go to **Settings → Pages**
4. Check **Enforce HTTPS** (appears after DNS verification)

---

## 📁 Supported File Types

GitHub Pages supports:
- **HTML** - `.html` files
- **Markdown** - `.md` files (with Jekyll)
- **CSS** - Stylesheets
- **JavaScript** - Client-side scripts
- **Images** - PNG, JPG, GIF, SVG
- **JSON** - Data files

### Jekyll Processing
By default, GitHub Pages processes files through Jekyll:
- Markdown files are converted to HTML
- YAML front matter is processed
- Liquid templating is available

To disable Jekyll (for pure static sites):
- Add an empty `.nojekyll` file to the root

---

## ⚙️ Advanced Configuration

### Custom 404 Page

Create `404.html` in your root:
```html
<!DOCTYPE html>
<html>
<head>
	<title>Page Not Found</title>
</head>
<body>
	<h1>404 - Page Not Found</h1>
	<p><a href="/">Return to homepage</a></p>
</body>
</html>
```

### Redirects

For redirects, use meta refresh or JavaScript:
```html
<meta http-equiv="refresh" content="0;url=https://ansarya.com/new-page">
```

### Subdomains

To use a subdomain (e.g., `blog.ansarya.com`):
1. Add CNAME record in DNS: `blog → theansarya.github.io`
2. Update CNAME file in repo: `blog.ansarya.com`
3. Wait for DNS propagation

---

## 🚨 Troubleshooting

### "404 - There isn't a GitHub Pages site here"

**Causes:**
- Pages not enabled in settings
- Wrong branch/folder configured
- Missing index.html
- CNAME file incorrect

**Solutions:**
1. Verify Pages is enabled in Settings → Pages
2. Check branch and folder settings
3. Ensure index.html exists
4. Verify CNAME contains only the domain name

### "DNS check failed"

**Causes:**
- DNS not propagated
- Incorrect DNS records
- CNAME file missing or wrong

**Solutions:**
1. Wait 24-48 hours for propagation
2. Verify DNS at [whatsmydns.net](https://www.whatsmydns.net/#A/ansarya.com)
3. Check CNAME file contents
4. Verify A records point to GitHub IPs

### "HTTPS not available"

**Causes:**
- DNS still propagating
- Certificate not yet issued
- CAA record blocking

**Solutions:**
1. Wait up to 24 hours
2. Remove and re-add custom domain
3. Check CAA records allow Let's Encrypt

### Build Failures

**Check:**
1. Actions tab for error details
2. Jekyll configuration (if using)
3. File encoding (must be UTF-8)
4. Invalid characters in filenames

---

## 📊 Monitoring & Analytics

### Deployment Status
- Check **Actions** tab for build/deploy status
- Enable notifications for failed deployments

### Traffic Analytics
Options for analytics:
- GitHub traffic insights (Settings → Traffic)
- Google Analytics
- Plausible Analytics
- Cloudflare Analytics (if using Cloudflare)

---

## 🔗 Related Documentation

- [Custom Domain Setup](custom-domain-setup.md) - Full DNS configuration
- [Cloudflare Settings](cloudflare-settings.md) - CDN and SSL configuration
- [Marketplace Setup Guide](marketplace-setup-guide.md) - VS Code extension publishing

---

## 📚 External Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Configuring Custom Domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [GitHub Pages IP Addresses](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain)
- [Troubleshooting Custom Domains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages)

---

*Last Updated: January 16, 2026*
