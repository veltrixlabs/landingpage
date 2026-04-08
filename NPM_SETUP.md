# Veltrix Labs Landing Page - npm Setup Guide

## 🚀 Quick Start (3 Commands)

```bash
# 1. Run locally
npm start

# 2. Push to GitHub
git push origin main

# 3. Watch it deploy automatically to Cloudflare Pages
# (No more steps needed - it's automatic!)
```

## 📦 What's Included

This landing page comes with complete npm integration:

- **`package.json`** - npm scripts and metadata
- **`wrangler.toml`** - Cloudflare Pages configuration
- **`_redirects`** - URL routing for Cloudflare Pages
- **`.gitignore`** - Proper git exclusions
- **`.github/workflows/`** - Optional GitHub Actions (you can add if needed)

## 🎯 Available npm Commands

```bash
npm start              # Start development server (localhost:8000)
npm run dev            # Same as npm start
npm run serve          # Alternative server using http-server
npm run build          # Validate and prepare for deployment
npm run validate       # Check all files are present
npm run deploy:netlify # Deploy to Netlify (optional)
npm run deploy:vercel  # Deploy to Vercel (optional)
npm run deploy:cloudflare  # Deploy with Wrangler CLI (optional)
```

## ⚡ Local Development

```bash
# Install dependencies (optional - downloads CLI tools)
npm install

# Start development server
npm start

# Open browser to: http://localhost:8000
# The server auto-refreshes if you edit files
```

## 🔗 GitHub + Cloudflare Pages Setup (The Easy Way)

### Prerequisites
- GitHub account with this repo
- Cloudflare account (free at cloudflare.com)

### 5-Minute Setup

1. **Push your code to GitHub:**
   ```bash
   git add veltrix-labs/landing/
   git commit -m "Add landing page"
   git push origin main
   ```

2. **Go to Cloudflare Dashboard:**
   - Visit https://dash.cloudflare.com
   - Navigate to **Pages**
   - Click **Create a project**
   - Select **Connect to Git**

3. **Authorize GitHub:**
   - Click "Connect GitHub"
   - Select your repository
   - Click "Begin setup"

4. **Configure Build Settings:**
   - **Project name:** `veltrix-labs-landing`
   - **Production branch:** `main`
   - **Framework preset:** `None` (static site)
   - **Build command:** `npm run build`
   - **Build output directory:** `.`
   - **Root directory (under Advanced):** `veltrix-labs/landing`
   - Click **Save and Deploy**

5. **That's it!** ✅
   - Your site is now live
   - Every `git push` to main automatically redeploys
   - Cloudflare will show you a preview URL

6. **Connect your domain (optional):**
   - After first deploy succeeds
   - Go to **Project Settings** → **Domains**
   - Click **Add custom domain**
   - Enter `www.veltrixlabs.net`
   - Update DNS records (instructions provided)

## 🔄 How the Automation Works

When you `git push origin main`:

1. **GitHub notifies Cloudflare** that code changed
2. **Cloudflare pulls your repo**
3. **Runs `npm run build`** to validate the site
4. **Deploys to Cloudflare's global CDN**
5. **Your site updates automatically** (30 seconds)

No manual steps. No button clicking. Just `git push` and you're done! 🎉

## 📝 Files & Configuration

### `package.json`
Contains npm scripts and deployment tool declarations.

### `wrangler.toml`
Cloudflare Pages configuration (already set up):
- App name: `veltrix-labs-landing`
- Build command: `npm run build`
- Build output: current directory

### `_redirects`
Routes all URLs to index.html (for proper SPA routing):
```
/* /index.html 200
```

### `.gitignore`
Excludes build artifacts and local dependencies from git.

## 🧪 Testing Locally Before Deploying

```bash
# 1. Make a change to the landing page
nano index.html

# 2. Test it works locally
npm start
# Visit http://localhost:8000 to verify

# 3. Validate everything is correct
npm run validate
# Output: "All files validated successfully!"

# 4. Commit and push
git add veltrix-labs/landing/
git commit -m "Landing page update: [describe change]"
git push origin main

# 5. Watch deployment in Cloudflare Dashboard
# Should be live in ~30 seconds
```

## 🛠️ Alternative Deployment Methods

If you prefer other platforms, npm scripts are ready:

### Deploy to Netlify
```bash
npm install -g netlify-cli
npm run deploy:netlify
```

### Deploy to Vercel
```bash
npm install -g vercel
npm run deploy:vercel
```

### Deploy with Wrangler (Manual Cloudflare)
```bash
npm install -g wrangler
wrangler login
npm run deploy:cloudflare
```

## ❓ FAQ

**Q: Do I need to run `npm install` every time?**
A: No, only once. The dependencies are cached locally in `node_modules/`.

**Q: What if the build fails?**
A: Check Cloudflare Pages build logs. Usually it's a file path issue in the configuration.

**Q: Can I preview changes before deploying?**
A: Yes! `npm start` runs locally so you can test first.

**Q: How do I rollback a deployment?**
A: Go to Cloudflare Pages dashboard and revert to a previous deployment with one click.

**Q: Can I add custom domains?**
A: Yes! After your first deployment, go to Project Settings → Domains to add `www.veltrixlabs.net`.

**Q: Is there a cost?**
A: Cloudflare Pages is **free** including unlimited deployments and bandwidth.

## 📚 Reference Links

- Cloudflare Pages Docs: https://developers.cloudflare.com/pages/
- wrangler CLI: https://developers.cloudflare.com/workers/wrangler/
- GitHub Integration: https://developers.cloudflare.com/pages/platform/github-integration/

## 💡 Next Steps

1. ✅ Run `npm start` to test locally
2. ✅ Push to GitHub: `git push origin main`
3. ✅ Connect to Cloudflare Pages (5 min setup above)
4. ✅ Watch automatic deployments on every push
5. ✅ Connect your custom domain `www.veltrixlabs.net`

---

**Happy deploying!** 🚀

For detailed deployment instructions, see [DEPLOYMENT.md](DEPLOYMENT.md)
