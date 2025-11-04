# Deploy Web App with YouTube Cookies

## 🌐 For Public Web Users

When you deploy this app publicly, **all users share the same server cookies**. This means:

- ✅ Users don't need to provide cookies
- ✅ Works for everyone automatically
- ⚠️ You (the server owner) provide one set of cookies
- ⚠️ All downloads use your YouTube session

---

## 🚀 Quick Setup for Deployment

### Step 1: Get Your Cookies Locally First

```bash
# 1. Export cookies using browser extension
# (See YOUTUBE-BOT-FIX.md for details)

# 2. Test locally
python test_cookies.py

# Should show: ✓ SUCCESS!
```

### Step 2: Encode Cookies for Deployment

```bash
# Convert cookies to base64 (one line, safe for environment variables)
base64 cookies/youtube.txt | tr -d '\n' > cookies_encoded.txt

# Copy the content
cat cookies_encoded.txt
```

### Step 3: Add to Your Platform

Choose your platform:

---

## 🎨 Render.com Deployment

### Method A: Environment Variable (Recommended)

1. **Go to your Render dashboard**
2. **Select your web service**
3. **Go to Environment**
4. **Add New Environment Variable:**
   - **Key:** `YOUTUBE_COOKIES_BASE64`
   - **Value:** (paste content from `cookies_encoded.txt`)
5. **Click "Save Changes"**
6. **Wait for redeploy** (~2 minutes)

### Method B: Secret Files

1. **Go to Environment → Files**
2. **Add Secret File:**
   - **Filename:** `cookies/youtube.txt`
   - **Contents:** (paste original cookies.txt content)
3. **Save and redeploy**

---

## 🚂 Railway Deployment

### Using Environment Variables

1. **Go to your Railway project**
2. **Click Variables**
3. **Add Variable:**
   - **Name:** `YOUTUBE_COOKIES_BASE64`
   - **Value:** (paste from `cookies_encoded.txt`)
4. **Deploy** (automatic)

---

## 🐳 Fly.io Deployment

### Using Secrets

```bash
# Set the secret
flyctl secrets set YOUTUBE_COOKIES_BASE64="$(base64 cookies/youtube.txt | tr -d '\n')"

# Deploy
flyctl deploy
```

---

## 🐳 Docker Deployment

### Option A: Build-time (for private images)

```dockerfile
# In Dockerfile, add:
COPY cookies/youtube.txt /app/cookies/youtube.txt
```

### Option B: Runtime (better security)

```bash
# Run with environment variable
docker run -e YOUTUBE_COOKIES_BASE64="$(base64 cookies/youtube.txt | tr -d '\n')" your-image

# Or with docker-compose
```

Add to `docker-compose.yml`:
```yaml
services:
  web:
    environment:
      - YOUTUBE_COOKIES_BASE64=${YOUTUBE_COOKIES_BASE64}
```

Then create `.env`:
```bash
YOUTUBE_COOKIES_BASE64=<paste-encoded-cookies>
```

---

## ✅ Verify Deployment

After deploying:

1. **Visit your deployed URL**
2. **Try to download a video**
3. **Should work!** ✨

If you get bot errors:
- Check environment variable is set correctly
- Check cookies aren't expired
- Re-export and re-deploy

---

## 🔄 When Cookies Expire (Every 2-4 Weeks)

### Quick Refresh

```bash
# 1. Export fresh cookies from browser
# 2. Encode them
base64 cookies/youtube.txt | tr -d '\n' > cookies_encoded.txt

# 3. Update environment variable on your platform
# 4. Redeploy (usually automatic)
```

### Automation (Advanced)

You could create a cron job to:
1. Check if downloads are failing
2. Send you a notification
3. Manually refresh cookies

---

## 🛡️ Security Best Practices

### 1. Use Your Own Account

- Create a separate Google account
- Use it only for this service
- Don't use your personal account

### 2. Limit Access

Consider adding:
- Rate limiting (already included)
- IP whitelist (for private use)
- Authentication (password protect)
- Usage monitoring

### 3. Monitor Activity

- Check YouTube download history
- Watch for unusual activity
- Rotate cookies regularly

---

## 📊 How It Works

```
User Browser → Your Server (with cookies) → YouTube
                      ↓
              Uses YOUR session
              All downloads as "you"
```

**This means:**
- ✅ Users don't configure anything
- ✅ Works for everyone
- ⚠️ Uses your YouTube quota
- ⚠️ Downloads appear in your history

---

## ⚖️ Legal & Ethical Considerations

### Important Notes:

1. **YouTube ToS:** Downloading may violate Terms of Service
2. **Quota:** YouTube may rate-limit your account
3. **Privacy:** All downloads use your session
4. **Responsibility:** You're responsible for server usage

### Recommendations:

- ✅ Use for personal/educational purposes
- ✅ Create dedicated account for service
- ✅ Monitor usage regularly
- ✅ Consider rate limiting
- ❌ Don't use for commercial purposes
- ❌ Don't share widely without monitoring

---

## 🔧 Alternative: User-Provided Cookies (Advanced)

Instead of server-side cookies, you could:

### Option 1: Let Users Upload Cookies

**Pros:**
- Users use their own sessions
- No central account needed
- Better privacy

**Cons:**
- Complex for users
- Need to store cookies per session
- Security concerns

**Implementation:**
- Add file upload in web UI
- Store in session/database
- Use per-user cookies

### Option 2: OAuth Authentication

**Pros:**
- Official method
- No cookies needed
- Better security

**Cons:**
- Requires Google API setup
- More complex
- API quotas

### Option 3: Proxy Service

Use a proxy service that handles authentication:
- More expensive
- Less control
- But easier for users

---

## 📋 Quick Command Reference

### Encode cookies:
```bash
base64 cookies/youtube.txt | tr -d '\n' > cookies_encoded.txt
```

### Test locally:
```bash
export YOUTUBE_COOKIES_BASE64="$(base64 cookies/youtube.txt | tr -d '\n')"
python app.py
```

### Verify environment variable:
```bash
# Render
curl https://your-app.onrender.com/health

# Railway
railway run echo $YOUTUBE_COOKIES_BASE64

# Fly.io
flyctl ssh console -C "env | grep YOUTUBE"
```

---

## 🆘 Troubleshooting

### "Bot detection" error on deployed app

1. **Check env var is set:**
   - Go to platform dashboard
   - Verify `YOUTUBE_COOKIES_BASE64` exists
   - Check it's not truncated

2. **Re-encode and update:**
   ```bash
   base64 cookies/youtube.txt | tr -d '\n'
   # Copy and update env var
   ```

3. **Check cookies are fresh:**
   - Export new cookies
   - Update env var
   - Redeploy

### Works locally but not deployed

- Environment variable name must be exact: `YOUTUBE_COOKIES_BASE64`
- Value must be base64-encoded (no newlines)
- Redeploy after adding env var
- Check platform logs for errors

### Downloads work but slow/rate limited

- YouTube may be rate-limiting your account
- Consider:
  - Using multiple accounts (rotate)
  - Adding delays between downloads
  - Monitoring usage

---

## 🎯 Recommended Setup

**For personal use (you + friends):**
1. Use single account cookies via environment variable
2. Monitor usage occasionally
3. Refresh cookies monthly

**For public service:**
1. Consider user authentication
2. Add rate limiting per IP
3. Monitor closely
4. Be prepared for YouTube rate limits
5. Have backup cookies ready

**For production:**
1. Use dedicated Google account
2. Implement OAuth (official method)
3. Add comprehensive rate limiting
4. Monitor and log everything
5. Have legal disclaimer

---

## 📖 Summary

**Quick steps:**
1. ✅ Export cookies locally (browser extension)
2. ✅ Test locally (`python test_cookies.py`)
3. ✅ Encode cookies (`base64 cookies/youtube.txt | tr -d '\n'`)
4. ✅ Add to deployment (environment variable `YOUTUBE_COOKIES_BASE64`)
5. ✅ Deploy and test
6. ✅ Refresh every 2-4 weeks

**All users will use your cookies automatically!** 🎉

---

## 🔗 Related Docs

- [YOUTUBE-BOT-FIX.md](YOUTUBE-BOT-FIX.md) - How to get cookies
- [FREE-HOSTING.md](FREE-HOSTING.md) - Deployment platforms
- [RENDER-QUICKSTART.md](RENDER-QUICKSTART.md) - Render deployment

---

**Your web users will never see bot errors!** 🚀
