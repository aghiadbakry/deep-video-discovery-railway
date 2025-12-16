# Deep Video Discovery - Railway Deployment

This directory contains the Railway-specific deployment configuration for Deep Video Discovery.

## 🚀 Quick Deploy to Railway

### Prerequisites
- Railway account (sign up at https://railway.app)
- GitHub account (for connecting your repository)
- OpenAI API key

### Deployment Steps

1. **Push to GitHub**
   ```bash
   cd railway
   git init
   git add .
   git commit -m "Initial Railway deployment"
   git remote add origin <your-github-repo-url>
   git push -u origin main
   ```

2. **Create Railway Project**
   - Go to https://railway.app
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Choose your repository
   - Railway will auto-detect Python and deploy

3. **Set Environment Variables**
   In Railway Dashboard → Your Service → Variables:
   - `OPENAI_API_KEY`: Your OpenAI API key
   - `YOUTUBE_COOKIES_B64`: (Optional) Base64-encoded YouTube cookies for better bot detection bypass
   - `VIDEO_DATABASE_FOLDER`: (Optional) Defaults to `/tmp/video_database/`

4. **Deploy**
   - Railway will automatically build and deploy
   - Your app will be available at `https://<your-app>.railway.app`

## 🔧 Configuration

### Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `OPENAI_API_KEY` | Yes | Your OpenAI API key |
| `YOUTUBE_COOKIES_B64` | No | Base64-encoded YouTube cookies (see below) |
| `VIDEO_DATABASE_FOLDER` | No | Storage path (default: `/tmp/video_database/`) |
| `PORT` | Auto | Railway sets this automatically |

### YouTube Cookies Setup (Optional but Recommended)

To reduce YouTube bot detection:

1. **Export cookies from browser:**
   - Install "Get cookies.txt LOCALLY" extension
   - Go to YouTube (while signed in)
   - Export cookies as `youtube_cookies.txt`

2. **Convert to base64:**
   ```bash
   # Windows PowerShell
   [Convert]::ToBase64String([IO.File]::ReadAllBytes("youtube_cookies.txt"))
   
   # Mac/Linux
   base64 youtube_cookies.txt | tr -d '\n'
   ```

3. **Add to Railway:**
   - Copy the entire base64 string (no line breaks)
   - Add as `YOUTUBE_COOKIES_B64` environment variable

## 📁 Directory Structure

```
railway/
├── app.py                 # Main Gradio application
├── requirements.txt       # Python dependencies
├── railway.json          # Railway configuration
├── nixpacks.toml         # Build configuration
├── README.md             # This file
├── DEPLOY.md             # Detailed deployment guide
└── dvd/                  # Core DVD modules
    ├── config.py
    ├── dvd_core.py
    ├── video_utils.py
    └── ...
```

## 🆚 Railway vs Render

**Railway Advantages:**
- ✅ Better network connectivity (may help with YouTube)
- ✅ More flexible runtime environment
- ✅ Easier environment variable management
- ✅ Better logging and debugging tools
- ✅ Free tier with $5 credit monthly

**Note:** Railway won't solve YouTube bot detection completely, but different IP ranges might help.

## 🐛 Troubleshooting

### YouTube Bot Detection
- Ensure `YOUTUBE_COOKIES_B64` is set with fresh cookies
- Cookies expire - refresh them periodically
- See `COOKIE_TROUBLESHOOTING.md` for details

### Build Failures
- Check Railway logs for specific errors
- Ensure `requirements.txt` is correct
- Verify Python version compatibility

### Port Issues
- Railway sets `PORT` automatically - don't override it
- App listens on `0.0.0.0` to accept external connections

## 📚 Additional Resources

- [Railway Documentation](https://docs.railway.app)
- [Gradio Documentation](https://gradio.app/docs)
- [yt-dlp Documentation](https://github.com/yt-dlp/yt-dlp)

## 🔄 Updating Deployment

Railway auto-deploys on git push:
```bash
git add .
git commit -m "Update deployment"
git push
```

Railway will automatically rebuild and redeploy.
