# VS Code Marketplace Setup Guide

This guide walks you through publishing your VS Code extension to the Visual Studio Marketplace.

## Prerequisites

1. **Microsoft Account**: You need a Microsoft account to create a publisher
2. **Azure DevOps Organization**: Required for creating publisher and Personal Access Token (PAT)
3. **Extension Package**: Your extension must be packaged as a `.vsix` file

## Step 1: Create a Publisher

1. Go to [Visual Studio Marketplace Publisher Management](https://marketplace.visualstudio.com/manage)
2. Sign in with your Microsoft account
3. Click "Create publisher"
4. Fill in the required information:
	- **Publisher ID**: `TheAnsarya` (must be unique, lowercase, no spaces)
	- **Display Name**: Your display name
	- **Email**: Your contact email
	- **Website**: https://theansarya.github.io/ansarya/

## Step 2: Create a Personal Access Token (PAT)

1. Go to [Azure DevOps](https://dev.azure.com)
2. Sign in with the same Microsoft account
3. Click on User Settings (top right) → Personal Access Tokens
4. Click "+ New Token"
5. Configure the token:
	- **Name**: "VS Code Marketplace Publishing"
	- **Organization**: All accessible organizations
	- **Expiration**: 90 days (or custom)
	- **Scopes**: Custom defined
	 - **Marketplace**: Select "Acquire" and "Manage"
6. Click "Create" and **copy the token** (you won't see it again!)

## Step 3: Install vsce (Visual Studio Code Extension Manager)

```bash
npm install -g @vscode/vsce
```

## Step 4: Create package.json for Your Extension

If you haven't already, create a `package.json` file in your extension root:

```json
{
	"name": "your-extension-name",
	"displayName": "Your Extension Display Name",
	"description": "A brief description of your extension",
	"version": "0.0.1",
	"publisher": "TheAnsarya",
	"engines": {
	"vscode": "^1.85.0"
	},
	"categories": [
	"Other"
	],
	"activationEvents": [],
	"main": "./extension.js",
	"contributes": {},
	"repository": {
	"type": "git",
	"url": "https://github.com/TheAnsarya/ansarya.git"
	},
	"bugs": {
	"url": "https://github.com/TheAnsarya/ansarya/issues"
	},
	"homepage": "https://theansarya.github.io/ansarya/",
	"license": "SEE LICENSE IN LICENSE"
}
```

## Step 5: Package Your Extension

```bash
vsce package
```

This creates a `.vsix` file.

## Step 6: Publish Your Extension

### Option A: Using vsce Command Line

```bash
# Login with your PAT
vsce login TheAnsarya

# Publish
vsce publish
```

### Option B: Manual Upload

1. Go to [Marketplace Publisher Management](https://marketplace.visualstudio.com/manage/publishers/TheAnsarya)
2. Click "+ New extension" → "Visual Studio Code"
3. Drag and drop your `.vsix` file
4. Click "Upload"

## Step 7: Verify Domain for Publisher (Optional but Recommended)

This adds a verified badge to your publisher profile.

1. Go to your [Publisher Management Page](https://marketplace.visualstudio.com/manage/publishers/TheAnsarya)
2. Click on your publisher name
3. Scroll to "Verified Domain"
4. Enter your domain (e.g., `ansarya.com`)
5. You'll be given a TXT record to add to your DNS:
	- Type: `TXT`
	- Name: `@` or your root domain
	- Value: The verification string provided by Microsoft

### Using Cloudflare DNS for Verification

1. Log in to Cloudflare
2. Select your domain
3. Go to DNS → Records
4. Click "Add record"
5. Set:
	- Type: `TXT`
	- Name: `@` (for root domain) or `_vscode-marketplace` (if specified)
	- Content: The verification string from Microsoft
	- TTL: Auto
	- Proxy status: DNS only (gray cloud)
6. Save and wait for DNS propagation (can take a few minutes to 24 hours)
7. Return to Marketplace and click "Verify"

## GitHub Pages Setup with Custom Domain (Cloudflare)

### 1. Create GitHub Repository and Enable Pages

✅ You already have the repository: `TheAnsarya/ansarya`

To enable GitHub Pages:
1. Go to repository Settings
2. Navigate to Pages (left sidebar)
3. Under "Build and deployment":
	- Source: Deploy from a branch
	- Branch: `main`
	- Folder: `/pages` (or `/root` if your index.html is in root)
4. Click Save

GitHub will automatically deploy your site to: `https://theansarya.github.io/ansarya/`

### 2. Add CNAME File

The CNAME file has been created in the root of your repository. Update it with your actual domain:

```
ansarya.com
```

Or if using a subdomain:

```
www.ansarya.com
```

Commit and push the CNAME file.

### 3. Sign Up for Cloudflare

1. Go to [Cloudflare](https://www.cloudflare.com)
2. Create a free account
3. Add your domain
4. Cloudflare will scan your existing DNS records
5. Copy the Cloudflare nameservers (e.g., `ns1.cloudflare.com`, `ns2.cloudflare.com`)
6. Go to your domain registrar (where you bought the domain)
7. Update the nameservers to Cloudflare's nameservers
8. Wait for propagation (can take up to 24 hours, usually faster)

### 4. Configure DNS Records in Cloudflare

Once your domain is active on Cloudflare:

1. Go to DNS → Records
2. Add/Update these records:

**For apex domain (ansarya.com):**
```
Type: A
Name: @
IPv4 address: 185.199.108.153
Proxy status: Proxied (orange cloud)
TTL: Auto
```

Add these additional A records (GitHub Pages IPs):
```
185.199.109.153
185.199.110.153
185.199.111.153
```

**For www subdomain:**
```
Type: CNAME
Name: www
Target: theansarya.github.io
Proxy status: Proxied (orange cloud)
TTL: Auto
```

### 5. Configure Cloudflare SSL/TLS Settings

1. Go to SSL/TLS → Overview
2. Set encryption mode to **"Full"** or **"Full (strict)"**
3. Go to SSL/TLS → Edge Certificates
4. Enable:
	- ✅ Always Use HTTPS
	- ✅ Automatic HTTPS Rewrites
	- ✅ Minimum TLS Version: 1.2 (recommended)

Cloudflare will automatically provision a Universal SSL certificate (this can take 5 minutes to 24 hours).

### 6. Additional Cloudflare Settings (Optional)

**Speed Optimization:**
- Speed → Optimization
	- ✅ Auto Minify (HTML, CSS, JS)
	- ✅ Brotli compression

**Caching:**
- Caching → Configuration
	- Browser Cache TTL: 4 hours (or as preferred)

**Page Rules (Optional):**
Create a page rule to force HTTPS:
- URL: `http://*ansarya.com/*`
- Setting: Always Use HTTPS

### 7. Update GitHub Pages Settings

After DNS is configured:

1. Go to your GitHub repository Settings → Pages
2. Under "Custom domain", enter your domain: `ansarya.com`
3. Wait for DNS check to complete
4. ✅ Check "Enforce HTTPS" (may take a few minutes to become available)

### 8. Verify Everything Works

1. Visit `https://ansarya.com` - should load your site
2. Visit `https://www.ansarya.com` - should redirect/load your site
3. Visit `http://ansarya.com` - should redirect to HTTPS
4. Check SSL certificate - should show valid Cloudflare certificate

### Troubleshooting

**DNS not propagating:**
- Check DNS propagation: https://www.whatsmydns.net/
- Wait up to 24 hours for full propagation

**"404 There isn't a GitHub Pages site here":**
- Verify CNAME file contains correct domain
- Check GitHub Pages settings
- Ensure DNS records point to correct IPs

**SSL certificate not working:**
- Wait for Cloudflare to provision certificate (up to 24 hours)
- Ensure Proxy status is enabled (orange cloud)
- Check SSL/TLS mode is set to "Full"

**"HTTPS not available" in GitHub:**
- Wait 24 hours after adding custom domain
- Remove and re-add custom domain in GitHub settings
- Ensure CAA records allow Cloudflare certificates

## Useful Commands

```bash
# Package extension
vsce package

# Publish new version
vsce publish

# Publish patch version (0.0.1 → 0.0.2)
vsce publish patch

# Publish minor version (0.0.1 → 0.1.0)
vsce publish minor

# Publish major version (0.0.1 → 1.0.0)
vsce publish major

# Unpublish extension (cannot be undone!)
vsce unpublish TheAnsarya.your-extension-name

# Show extension info
vsce show TheAnsarya.your-extension-name
```

## Important Files for Marketplace

Ensure these files are in your extension root:

- ✅ `package.json` - Extension manifest
- ✅ `README.md` - Extension documentation (shown in marketplace)
- ✅ `LICENSE` - License file
- ✅ `CHANGELOG.md` - Version history (optional but recommended)
- ✅ Icon file (128x128 PNG) - Specified in package.json as `"icon": "icon.png"`

## Resources

- [VS Code Extension API](https://code.visualstudio.com/api)
- [Publishing Extensions](https://code.visualstudio.com/api/working-with-extensions/publishing-extension)
- [Marketplace Publisher Management](https://marketplace.visualstudio.com/manage)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Cloudflare DNS Documentation](https://developers.cloudflare.com/dns/)

---

**Note**: The website at https://theansarya.github.io/ansarya/ is now live and ready to be used as your publisher homepage and for domain verification!
