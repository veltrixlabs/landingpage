# Veltrix Labs - Production Deployment Guide

## Quick Start with npm

### Prerequisites
- Node.js 18+ (for npm commands)
- Domain name (optional but recommended)
- GitHub account (for Cloudflare Pages integration)

### Local Development

```bash
# Install dependencies (optional, mostly for deployment tools)
npm install

# Start development server
npm start
# or
npm run dev

# Server runs at http://localhost:8000
```

### Validation

```bash
# Validate all files are present and correct
npm run validate

# Output: "All files validated successfully!"
```

### Build (for CI/CD)

```bash
# Build command (used by Cloudflare Pages automatically)
npm run build
```

---

## Environment Setup

### Prerequisites
- Domain name (optional but recommended)
- SSL certificate (free with most platforms)
- DNS access for custom domain

## Deployment Options

### Option 1: Netlify (Recommended - Zero Configuration)

**Pros:** Free, fast, automatic HTTPS, CI/CD included

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login to Netlify
netlify login

# Deploy from landing folder
cd /path/to/veltrix-labs/landing
netlify deploy --prod
```

### Option 2: Vercel

**Pros:** Fast, serverless-ready, great UX

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

### Option 3: Cloudflare Pages (Recommended - Ultra-Fast Global CDN)

**Pros:** Free, blazing fast, global CDN, excellent DDoS protection, zero-config, automatic from GitHub

#### Method A: GitHub Integration (Recommended - Fully Automatic CI/CD)

**This is the easiest path. Everything happens automatically!**

1. **Push your code to GitHub:**
   ```bash
   # From your repo root
   git add veltrix-labs/landing/
   git commit -m "Add Veltrix Labs landing page"
   git push origin main
   ```

2. **Connect GitHub to Cloudflare Pages:**
   - Go to [Cloudflare Dashboard](https://dash.cloudflare.com)
   - Click **Pages** in the sidebar
   - Click **Create a project** → **Connect to Git**
   - Authorize Cloudflare with GitHub
   - Select your repository
   - Click **Begin setup**

3. **Configure build settings:**
   - **Project name:** `veltrix-labs-landing`
   - **Production branch:** `main`
   - **Framework preset:** None (static site)
   - **Build command:** `npm run build`
   - **Build output directory:** `.` (current folder)
   - **Root directory (advanced):** `veltrix-labs/landing`

4. **Save and Deploy:**
   - Click **Save and Deploy**
   - Cloudflare builds and deploys automatically
   - Your site is live at a Cloudflare subdomain instantly

5. **Connect your domain:**
   - After deployment succeeds, go to **Project Settings** → **Domains**
   - Click **Add a custom domain**
   - Enter: `www.veltrixlabs.net`
   - Follow nameserver instructions OR use CNAME record

**That's it! Every push to GitHub automatically builds and deploys.**

#### Method A.1: Optional - GitHub Actions for Preview Deployments

For pull request previews, create `.github/workflows/deploy-cloudflare.yml`:

```yaml
name: Deploy to Cloudflare Pages

on:
  push:
    branches: [main]
    paths:
      - 'veltrix-labs/landing/**'
  pull_request:
    paths:
      - 'veltrix-labs/landing/**'

jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm install
        working-directory: veltrix-labs/landing

      - name: Validate build
        run: npm run validate
        working-directory: veltrix-labs/landing

      - name: Deploy to Cloudflare Pages
        uses: cloudflare/pages-action@v1
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          projectName: veltrix-labs-landing
          directory: veltrix-labs/landing
          productionBranch: main
```

**Add GitHub Secrets:**
1. Go to GitHub repo → **Settings** → **Secrets and variables** → **Actions**
2. Create two new secrets:
   - `CLOUDFLARE_API_TOKEN` - From Cloudflare Profile → API Tokens → Create Token
   - `CLOUDFLARE_ACCOUNT_ID` - From Cloudflare Dashboard → bottom left corner

#### Method B: Using Wrangler CLI (Manual)

For manual deployments:

```bash
# Install Wrangler CLI
npm install -g wrangler

# Login to Cloudflare
wrangler login

# Deploy (from landing directory)
cd veltrix-labs/landing
wrangler publish
```

### Option 4: GitHub Pages (Free)

```bash
# Push code to GitHub
git add landing/
git commit -m "Add Veltrix Labs landing page"
git push origin main

# Enable Pages in GitHub Settings
# Go to Settings → Pages → Select 'main' branch, '/landing' folder
```

### Option 5: AWS CloudFront + S3

```bash
# Create S3 bucket
aws s3 mb s3://veltrix-labs-landing

# Upload files
aws s3 sync landing/ s3://veltrix-labs-landing/

# Create CloudFront distribution pointing to S3
# This provides CDN caching + HTTPS
```

### Option 6: Docker (For self-hosted)

```dockerfile
# Create Dockerfile in landing/ folder
FROM nginx:latest
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```bash
# Build and run
docker build -t veltrix-landing .
docker run -p 80:80 veltrix-landing
```

## Domain Setup

### Connect Custom Domain (Netlify Example)

1. Go to Netlify Dashboard
2. Site Settings → Domain Management
3. Add custom domain: www.veltrixlabs.net
4. Update nameservers or DNS records
5. Wait for propagation (24-48 hours)

### DNS Records
```
Type    Name              Value
A       veltrixlabs.net   Netlify IP
CNAME   www               veltrixlabs.net
```

## Performance Optimization

### Enable Caching
- **Static files (CSS, JS, Images)**: 1 year
- **HTML**: 24 hours (no-cache for old browsers)

### Set Cache Headers (Netlify)
```toml
# netlify.toml
[[headers]]
  for = "/*"
  [headers.values]
    Cache-Control = "public, max-age=86400"

[[headers]]
  for = "/index.html"
  [headers.values]
    Cache-Control = "public, max-age=3600"
```

### Enable Gzip Compression
Most platforms do this automatically. Verify:
- Netlify: ✓ Automatic
- Vercel: ✓ Automatic
- AWS S3 + CloudFront: ✓ Enable compression in CloudFront

## SSL/HTTPS

✓ All recommended platforms provide free HTTPS with auto-renewal
- Netlify: Automatic
- Vercel: Automatic
- GitHub Pages: Automatic
- AWS: Use Certificate Manager (free)

## Monitoring

### Setup Uptime Monitoring
```bash
# Option 1: UptimeRobot (free)
# Visit uptimerobot.com → Add monitor for your URL

# Option 2: Pingdom
# Setup alerts for downtime

# Option 3: CloudWatch (AWS)
# Configure synthetic endpoints
```

## Analytics Setup

### Google Analytics 4
1. Create GA4 property
2. Get Measurement ID
3. Add to landing page:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Microsoft Clarity
Add for heatmaps and user insights:
```html
<script type="text/javascript">
  (function(c,l,a,r,i,t,y){
    c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
    y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
  })(window, document, "clarity", "script", "YOUR_PROJECT_ID");
</script>
```

## Backup & Disaster Recovery

```bash
# Backup landing page regularly
cd /path/to/veltrix-labs
git add landing/
git commit -m "Backup landing page"
git push origin main
```

## Security Checklist

- [x] HTTPS enabled
- [x] No sensitive data in HTML
- [x] Form submissions to secure backend
- [x] CSP headers configured
- [ ] Add sitemap.xml
- [ ] Add robots.txt
- [ ] Set up monitoring
- [ ] Regular backups

## Post-Deployment

### 1. Test Everything
- [ ] Load speed (Google PageSpeed Insights)
- [ ] Mobile responsiveness
- [ ] All links work
- [ ] Forms submit correctly
- [ ] Analytics tracking works

### 2. Submit to Search Engines
```bash
# Google Search Console
https://search.google.com/search-console

# Bing Webmaster Tools
https://www.bing.com/webmasters

# Add robots.txt and sitemap.xml
```

### 3. Configure Email
- Setup contact form backend
- Configure transactional emails
- Setup CRM integration if needed

### 4. Social Media
- Add social sharing meta tags
- Update social profiles with landing page URL
- Schedule launch announcements

## Troubleshooting

### Page not loading
- Check build logs in deployment platform
- Verify all file paths are correct
- Check browser console for errors

### Slow performance
- Check Core Web Vitals
- Optimize images
- Enable caching headers
- Use CDN

### Form submissions not working
- Add backend API endpoint
- Verify CORS settings
- Check network tab in DevTools

## Quick Reference - npm Commands

```bash
# Run locally
npm start                    # Start dev server at localhost:8000
npm run dev                  # Same as npm start
npm run serve                # Alternative using http-server

# Validate files
npm run validate             # Check all required files exist

# Build (used by Cloudflare Pages automatically)
npm run build                # Validate and prepare for deployment

# One-liner deployment setup
npm run deploy:netlify       # Deploy to Netlify
npm run deploy:vercel        # Deploy to Vercel
npm run deploy:cloudflare    # Deploy with Wrangler CLI
```

## Complete Workflow: GitHub Push → Cloudflare Deploy

### Step-by-Step Setup (5 minutes)

**1. Prepare your code:**
```bash
cd /path/to/helios-crest
git add veltrix-labs/landing/
git commit -m "Add Veltrix Labs landing page with CI/CD"
git push origin main
```

**2. Connect Cloudflare to GitHub:**
- Go to https://dash.cloudflare.com
- Click **Pages** → **Create a project** → **Connect to Git**
- Select your GitHub repository
- Click **Begin setup**

**3. Configure build settings in Cloudflare:**
```
Project name:              veltrix-labs-landing
Production branch:         main
Framework preset:          None (static)
Build command:             npm run build
Build output directory:    .
Root directory:            veltrix-labs/landing  ← IMPORTANT
```

**4. Deploy:**
- Click **Save and Deploy**
- Watch the build complete (takes ~30 seconds)
- Get your Cloudflare URL

**5. Connect your domain:**
- Go to **Project Settings** → **Domains**
- **Add custom domain** → `www.veltrixlabs.net`
- Add nameservers or CNAME records in your DNS provider

**6. Done!** 🎉
Every `git push` to main automatically:
- ✓ Triggers build (runs `npm run build`)
- ✓ Validates files
- ✓ Deploys to Cloudflare's global CDN
- ✓ Updates your site instantly

### Verify it's working:
```bash
# Make a test change
echo "<!-- Test comment -->" >> veltrix-labs/landing/index.html
git add veltrix-labs/landing/index.html
git commit -m "Test deployment trigger"
git push origin main

# Watch deployment in Cloudflare Dashboard → Pages
# Should complete in ~30 seconds
```

## Support & Updates

For any questions or to request changes:
1. Create issue in repository
2. Contact DevOps team
3. Reference this deployment guide

---

**Last Updated:** April 2026
**Status:** Production Ready ✓
