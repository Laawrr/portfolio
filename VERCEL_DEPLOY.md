# How to Deploy Portfolio to Vercel

## Method 1: Deploy via Vercel Website (Easiest)

### Step 1: Push to GitHub First
1. Make sure your code is pushed to GitHub (see `GITHUB_UPLOAD.md`)
2. Your repository should be public or you need to grant Vercel access

### Step 2: Deploy on Vercel
1. Go to [vercel.com](https://vercel.com) and sign in (use GitHub to sign in)
2. Click **"Add New..."** → **"Project"**
3. Import your GitHub repository
4. Vercel will auto-detect SvelteKit settings:
   - **Framework Preset**: SvelteKit (auto-detected)
   - **Build Command**: `npm run build` (auto-detected)
   - **Output Directory**: `.svelte-kit` (auto-detected)
   - **Install Command**: `npm install` (auto-detected)
5. Click **"Deploy"**
6. Wait 1-2 minutes for deployment
7. Your site will be live at `your-project-name.vercel.app`

### Step 3: Custom Domain (Optional)
1. Go to your project settings on Vercel
2. Click **"Domains"**
3. Add your custom domain

---

## Method 2: Deploy via Vercel CLI

### Step 1: Install Vercel CLI
```powershell
npm install -g vercel
```

### Step 2: Login to Vercel
```powershell
vercel login
```

### Step 3: Deploy
```powershell
# In your project directory
vercel

# For production deployment
vercel --prod
```

---

## Method 3: Optimize for Vercel (Recommended)

### Step 1: Install Vercel Adapter
```powershell
npm install -D @sveltejs/adapter-vercel
```

### Step 2: Update svelte.config.js
Change:
```js
import adapter from '@sveltejs/adapter-auto';
```
To:
```js
import adapter from '@sveltejs/adapter-vercel';
```

### Step 3: Deploy
Follow Method 1 or 2 above.

---

## Automatic Deployments

Once connected to GitHub:
- **Every push to `main` branch** = Automatic production deployment
- **Pull requests** = Automatic preview deployments

---

## Environment Variables (if needed)

If you need environment variables:
1. Go to your project on Vercel
2. Settings → Environment Variables
3. Add your variables

---

## Notes:
- Vercel automatically detects SvelteKit projects
- Your site will be live in ~1-2 minutes
- Free tier includes unlimited deployments
- Custom domains are free
- HTTPS is automatic

