# Veltrix Labs - Production Deployment Guide

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

### Option 3: GitHub Pages (Free)

```bash
# Push code to GitHub
git add landing/
git commit -m "Add Veltrix Labs landing page"
git push origin main

# Enable Pages in GitHub Settings
# Go to Settings → Pages → Select 'main' branch, '/landing' folder
```

### Option 4: AWS CloudFront + S3

```bash
# Create S3 bucket
aws s3 mb s3://veltrix-labs-landing

# Upload files
aws s3 sync landing/ s3://veltrix-labs-landing/

# Create CloudFront distribution pointing to S3
# This provides CDN caching + HTTPS
```

### Option 5: Docker (For self-hosted)

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

## Support & Updates

For any questions or to request changes:
1. Create issue in repository
2. Contact DevOps team
3. Reference this deployment guide

---

**Last Updated:** April 2026
**Status:** Production Ready ✓
