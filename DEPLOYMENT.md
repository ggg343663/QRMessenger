# 🚀 Deployment Guide

**How to deploy QRMessenger to GitHub Pages and other platforms**

---

## GitHub Setup (Quick Start)

### Step 1: Create GitHub Repository
```bash
# If you don't have the repo yet
git init
git add .
git commit -m "Initial commit: QRMessenger v1.0"

# Create a new repository on GitHub:
# https://github.com/new
# Name it: QRMessenger
# Description: Secure end-to-end encryption and QR code sharing
```

### Step 2: Connect to GitHub
```bash
git remote add origin https://github.com/ggg343663/QRMessenger.git
git branch -M main
git push -u origin main
```

### Step 3: Enable GitHub Pages
1. Go to repository **Settings**
2. Click **Pages** (left sidebar)
3. Under "Source", select **Deploy from a branch**
4. Select branch: `main`
5. Select folder: `/01_QRMessenger`
6. Click **Save**

**Result**: Your app is now live at:  
🔗 `https://ggg343663.github.io/QRMessenger/`

---

## GitHub Pages Configuration

### Using root folder
If you want to serve from the root:

```bash
# Move all files from 01_QRMessenger/ to root
mv 01_QRMessenger/* .
git add .
git commit -m "Move app to root for GitHub Pages"
git push
```

Then in **Settings > Pages**: Select `/root`

### Using docs/ folder
Alternative method (some prefer this):

```bash
# Copy app to docs/ folder
mkdir -p docs
cp 01_QRMessenger/* docs/
git add .
git commit -m "Add docs folder for GitHub Pages"
git push
```

Then in **Settings > Pages**: Select `docs/`

---

## Deploy to Other Platforms

### Netlify (Recommended)
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod --dir=01_QRMessenger

# Follow prompts to authorize and deploy
```

**Result**: Your app at Netlify's domain  
Bonus: Automatic HTTPS, CDN, custom domain support

### Vercel
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
cd 01_QRMessenger
vercel --prod

# Follow prompts
```

### Firebase Hosting
```bash
# Install Firebase CLI
npm install -g firebase-tools

# Initialize
firebase login
firebase init hosting

# When prompted, select:
# - Public directory: 01_QRMessenger
# - Single-page app: No

# Deploy
firebase deploy
```

### AWS S3 + CloudFront
```bash
# Upload to S3
aws s3 sync 01_QRMessenger/ s3://my-qrmessenger-bucket/ --delete

# Invalidate CloudFront cache
aws cloudfront create-invalidation --distribution-id <ID> --paths "/*"
```

### Docker
```dockerfile
# Dockerfile
FROM nginx:alpine
COPY 01_QRMessenger/ /usr/share/nginx/html/
EXPOSE 80
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost/ || exit 1
```

Build and run:
```bash
docker build -t qrmessenger:latest .
docker run -p 80:8080 qrmessenger:latest
```

---

## Local Development

### Method 1: Python (Recommended)
```bash
cd 01_QRMessenger
python -m http.server 8000
# Open: http://localhost:8000
```

### Method 2: Node.js (http-server)
```bash
npx http-server 01_QRMessenger -p 8000
# Open: http://localhost:8000
```

### Method 3: PHP
```bash
cd 01_QRMessenger
php -S localhost:8000
```

### Method 4: VS Code Live Server
1. Install extension: **Live Server** by Ritwick Dey
2. Right-click `QRMessenger_v1.0.html`
3. Click **Open with Live Server**

---

## Configuration Files

### manifest.json
Already configured. Used by browsers for PWA installation.

```json
{
  "name": "QRMessenger v1.0",
  "short_name": "QRMessenger",
  "start_url": "./QRMessenger_v1.0.html",
  "display": "standalone",
  "icons": [...]
}
```

### Service Worker (sw.js)
Enables offline support. Already included.

### .gitignore
Excludes build files, logs, and secrets:

```
build/
*.log
.env
node_modules/
```

---

## Continuous Deployment (CD)

### GitHub Actions
Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./01_QRMessenger
```

Push to trigger automatic deployment.

### Netlify
```bash
# netlify.toml
[build]
  command = "echo 'Static site, no build needed'"
  publish = "01_QRMessenger"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

---

## Custom Domain

### On GitHub Pages
1. Go to **Settings > Pages**
2. Under "Custom domain", enter: `qrmessenger.example.com`
3. Create `CNAME` file in repository:
```
echo "qrmessenger.example.com" > 01_QRMessenger/CNAME
git add 01_QRMessenger/CNAME
git commit -m "Add custom domain"
git push
```

4. Update DNS at your registrar:
   - Add `CNAME` record pointing to `ggg343663.github.io`

---

## SSL/HTTPS

✅ **Automatic**: GitHub Pages, Netlify, Vercel all provide free HTTPS  
✅ **Recommended**: Always use HTTPS for any web deployment  
⚠️ **Local**: Use `http://localhost` for local testing (not encrypted)

---

## Performance Optimization

### 1. Minify HTML
```bash
npm install -g html-minifier
html-minifier --collapse-whitespace --remove-comments QRMessenger_v1.0.html > QRMessenger_v1.0.min.html
```

### 2. Compress Images
```bash
# PNGs
pngquant --ext .png --force 256 *.png

# SVGs
npm install -g svgo
svgo *.svg
```

### 3. Enable Caching
Add headers (Netlify example in `netlify.toml`):
```toml
[[headers]]
  for = "/*"
  [headers.values]
    Cache-Control = "public, max-age=3600"

[[headers]]
  for = "/*.html"
  [headers.values]
    Cache-Control = "public, max-age=300"
```

---

## Monitoring & Analytics

### GitHub Insights
- Go to **Insights** tab
- View traffic, clones, forks

### Web Analytics (Privacy-Friendly)
- **Plausible**: No cookies, GDPR-compliant
- **Fathom Analytics**: Privacy-first, lightweight
- **GoAccess**: Server-side log analysis

Don't use Google Analytics (QRMessenger is zero-tracking)

---

## Update & Maintenance

### Pushing Updates
```bash
# Make changes locally
# Update files as needed

# Commit and push
git add .
git commit -m "Update: description of changes"
git push origin main

# GitHub Pages auto-deploys within minutes
```

### Version Tagging
```bash
git tag -a v1.0 -m "Release v1.0"
git push origin v1.0

# Later, create releases on GitHub UI
# Attach files to release
```

---

## Troubleshooting

### GitHub Pages Not Updating
```bash
# Clear browser cache
# Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)

# Or wait 5-10 minutes for GitHub to rebuild
```

### Service Worker Issues
```bash
# If PWA cache is outdated:
# In DevTools > Application > Service Workers > Unregister

# Or clear all site data
# Settings > Clear browsing data > Cookies and site data
```

### CORS Issues
QRMessenger doesn't make external requests, so CORS shouldn't be an issue.  
If importing QR images, ensure they're on same domain.

---

## Rollback Procedure

```bash
# View commit history
git log --oneline

# Revert to previous commit
git revert <commit-hash>
git push origin main

# Or reset (destructive, use with caution)
# git reset --hard <commit-hash>
# git push origin main --force
```

---

## Backup & Archive

### Local Backup
```bash
git clone https://github.com/ggg343663/QRMessenger.git QRMessenger-backup
```

### Archive Old Versions
Already done in `archive/` folder:
- `archive/01_QRMessenger/` - v1.0, v1.1
- `archive/02_QRMessenger_Pro/` - v2.0 versions

---

## Summary

| Platform | Setup | Cost | HTTPS | CDN | Ease |
|----------|:-----:|:----:|:-----:|:---:|:----:|
| GitHub Pages | Easy | Free | ✅ | ✅ | ⭐⭐⭐⭐ |
| Netlify | Very Easy | Free | ✅ | ✅ | ⭐⭐⭐⭐ |
| Vercel | Easy | Free | ✅ | ✅ | ⭐⭐⭐⭐ |
| Firebase | Medium | Free | ✅ | ✅ | ⭐⭐⭐ |
| AWS S3 | Hard | Cheap | Via CF | ✅ | ⭐⭐ |
| Docker | Medium | Varies | Manual | ❌ | ⭐⭐⭐ |

**Recommendation**: Start with GitHub Pages (free, simple, fast) or Netlify (slightly more features).

---

## Next Steps

1. ✅ Create GitHub account (if not already done)
2. ✅ Create repository
3. ✅ Enable GitHub Pages
4. ✅ Share URL: `https://ggg343663.github.io/QRMessenger/`
5. ✅ Test all features on live site
6. ✅ Share in readme/docs/about section

🎉 **Done!** Your QRMessenger is now live and shareable.

