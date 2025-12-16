# Railway Deployment Guide

Complete step-by-step guide for deploying Deep Video Discovery on Railway.

## 📋 Prerequisites

1. **Railway Account**
   - Sign up at https://railway.app
   - Free tier includes $5 credit monthly

2. **GitHub Account**
   - For connecting your repository

3. **OpenAI API Key**
   - Get from https://platform.openai.com/api-keys

## 🚀 Deployment Steps

### Step 1: Prepare Your Code

1. **Navigate to railway directory:**
   ```bash
   cd railway
   ```

2. **Initialize Git (if not already):**
   ```bash
   git init
   git add .
   git commit -m "Initial Railway deployment"
   ```

3. **Create GitHub Repository:**
   - Go to https://github.com/new
   - Create a new repository
   - Don't initialize with README (we already have one)

4. **Push to GitHub:**
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git branch -M main
   git push -u origin main
   ```

### Step 2: Deploy on Railway

1. **Create New Project:**
   - Go to https://railway.app
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Authorize Railway to access your GitHub
   - Select your repository

2. **Railway Auto-Detection:**
   - Railway will detect Python automatically
   - It will use `nixpacks.toml` or `railway.json` for configuration
   - Build will start automatically

3. **Wait for Build:**
   - Railway will install dependencies from `requirements.txt`
   - Build typically takes 2-5 minutes
   - Watch the build logs for progress

### Step 3: Configure Environment Variables

1. **Go to Service Settings:**
   - Click on your service in Railway dashboard
   - Go to "Variables" tab

2. **Add Required Variables:**
   
   **OPENAI_API_KEY** (Required):
   ```
   sk-proj-...your-api-key...
   ```

   **YOUTUBE_COOKIES_B64** (Optional but Recommended):
   - See "YouTube Cookies Setup" section below
   - Add the base64-encoded cookies string

   **VIDEO_DATABASE_FOLDER** (Optional):
   ```
   /tmp/video_database/
   ```
   (This is the default, only change if needed)

3. **Save Variables:**
   - Railway will automatically redeploy when variables change

### Step 4: Configure Domain

1. **Generate Domain:**
   - Go to "Settings" → "Networking"
   - Click "Generate Domain"
   - Railway will create a `.railway.app` domain

2. **Custom Domain (Optional):**
   - Add your own domain in "Custom Domains"
   - Configure DNS as instructed

### Step 5: Verify Deployment

1. **Check Logs:**
   - Go to "Deployments" tab
   - Click on latest deployment
   - Check logs for:
     - ✅ "YouTube cookies loaded" (if cookies set)
     - ✅ "Running on local URL: http://0.0.0.0:PORT"
     - ✅ No errors

2. **Test Application:**
   - Visit your Railway domain
   - Try a simple YouTube video
   - Check if it processes correctly

## 🍪 YouTube Cookies Setup

### Why Cookies Help

YouTube's bot detection can block subtitle downloads. Cookies help authenticate requests.

### Export Cookies

1. **Install Browser Extension:**
   - Chrome: "Get cookies.txt LOCALLY"
   - Firefox: "Get cookies.txt LOCALLY"

2. **Export Cookies:**
   - Go to YouTube (make sure you're signed in)
   - Click the extension icon
   - Click "Export"
   - Save as `youtube_cookies.txt`

3. **Convert to Base64:**
   
   **Windows PowerShell:**
   ```powershell
   [Convert]::ToBase64String([IO.File]::ReadAllBytes("youtube_cookies.txt"))
   ```
   
   **Mac/Linux:**
   ```bash
   base64 youtube_cookies.txt | tr -d '\n'
   ```

4. **Add to Railway:**
   - Copy the ENTIRE output (it's one long string)
   - Paste into `YOUTUBE_COOKIES_B64` variable
   - No line breaks or spaces!

### Refresh Cookies

Cookies expire! Refresh them:
- Every 1-2 weeks
- When you get bot detection errors
- After signing out/in to YouTube

## 🔧 Railway Configuration Files

### `railway.json`
```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "python app.py",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

### `Procfile`
```
web: python app.py
```

**Note:** Railway will auto-detect Python and use `requirements.txt` automatically. The `Procfile` specifies the start command.

## 🐛 Troubleshooting

### Build Fails

**Error: "No module named 'gradio'"**
- Check `requirements.txt` includes all dependencies
- Verify Python version compatibility

**Error: "Port already in use"**
- Railway sets `PORT` automatically - don't override it
- Ensure `app.py` uses `os.environ.get("PORT", "7860")`

### Runtime Errors

**YouTube Bot Detection:**
- Set `YOUTUBE_COOKIES_B64` with fresh cookies
- Check logs for cookie loading messages
- Refresh cookies if expired

**Memory Issues:**
- Railway free tier has memory limits
- Consider upgrading plan if needed
- Optimize video processing settings

### Logs

**View Logs:**
- Railway Dashboard → Deployments → Latest → Logs
- Real-time logs available
- Check for error messages

## 💰 Railway Pricing

**Free Tier:**
- $5 credit monthly
- 500 hours of usage
- Perfect for testing

**Pro Plan ($20/month):**
- $20 credit monthly
- Unlimited usage
- Better performance

## 🔄 Updating Deployment

Railway auto-deploys on git push:

```bash
# Make changes
git add .
git commit -m "Update code"
git push

# Railway automatically rebuilds and redeploys
```

## 📊 Monitoring

**Metrics Available:**
- CPU usage
- Memory usage
- Network traffic
- Request count

**View Metrics:**
- Railway Dashboard → Service → Metrics

## 🆘 Support

- **Railway Docs:** https://docs.railway.app
- **Railway Discord:** https://discord.gg/railway
- **GitHub Issues:** Create an issue in your repo

## ✅ Deployment Checklist

- [ ] Code pushed to GitHub
- [ ] Railway project created
- [ ] Repository connected
- [ ] Build successful
- [ ] `OPENAI_API_KEY` set
- [ ] `YOUTUBE_COOKIES_B64` set (optional)
- [ ] Domain configured
- [ ] Application accessible
- [ ] Test video processed successfully

## 🎉 Success!

Your Deep Video Discovery app is now deployed on Railway!

Visit your Railway domain to start using it.

