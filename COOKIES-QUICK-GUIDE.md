# 🍪 Cookies Quick Reference - For Web Deployment

## 📋 TL;DR - Deploy in 5 Minutes

Your web users will **NOT** need to configure anything. You (the server owner) configure cookies **once**, and it works for everyone.

---

## 🎯 Quick Setup

### 1. Get Cookies (2 minutes)

```bash
# Install browser extension:
# Chrome: https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc
# Firefox: https://addons.mozilla.org/firefox/addon/cookies-txt/

# 1. Open YouTube.com
# 2. Click extension → Export
# 3. Save as cookies.txt
```

### 2. Encode for Deployment (30 seconds)

```bash
base64 cookies/youtube.txt | tr -d '\n' > cookies_encoded.txt
cat cookies_encoded.txt
# Copy this output ↑
```

### 3. Add to Platform (1 minute)

**Render:**
- Dashboard → Environment → Add Variable
- Name: `YOUTUBE_COOKIES_BASE64`
- Value: (paste encoded cookies)
- Save

**Railway:**
- Variables → New Variable
- Name: `YOUTUBE_COOKIES_BASE64`
- Value: (paste encoded cookies)
- Done (auto-deploys)

**Fly.io:**
```bash
flyctl secrets set YOUTUBE_COOKIES_BASE64="$(base64 cookies/youtube.txt | tr -d '\n')"
```

### 4. Done! ✅

Visit your app → Try downloading → Works for everyone! 🎉

---

## 🔄 Refresh (Every 2-4 Weeks)

When downloads stop working:

```bash
# 1. Export fresh cookies (same as step 1)
# 2. Encode again
base64 cookies/youtube.txt | tr -d '\n'

# 3. Update environment variable
# 4. Redeploy (usually automatic)
```

---

## ❓ FAQ

**Q: Do my users need to configure cookies?**
A: No! Only you (server owner) do this once.

**Q: Will everyone see my YouTube account?**
A: No. But downloads use your session internally.

**Q: How often do I update?**
A: Every 2-4 weeks when cookies expire.

**Q: Can I use my personal account?**
A: Better to create a separate account for this.

**Q: What if I don't add cookies?**
A: YouTube will show "bot detection" errors.

**Q: Is this secure?**
A: Yes, cookies stay on your server. But don't share them publicly.

---

## 🆘 Troubleshooting

### Bot error after deployment:

```bash
# 1. Verify env var exists:
#    Platform dashboard → Check YOUTUBE_COOKIES_BASE64

# 2. Re-encode (cookies might have newlines):
base64 cookies/youtube.txt | tr -d '\n'

# 3. Update and redeploy
```

### Works locally but not deployed:

- Check env var name is exactly: `YOUTUBE_COOKIES_BASE64`
- Check value isn't truncated
- Check app redeployed after adding var

---

## 📚 Full Documentation

- **[DEPLOY-WITH-COOKIES.md](DEPLOY-WITH-COOKIES.md)** - Complete guide
- **[YOUTUBE-BOT-FIX.md](YOUTUBE-BOT-FIX.md)** - How to get cookies
- **[FREE-HOSTING.md](FREE-HOSTING.md)** - Deployment platforms

---

## ✅ Checklist

For web deployment:
- [ ] Export cookies from browser
- [ ] Encode with base64 (no newlines)
- [ ] Add `YOUTUBE_COOKIES_BASE64` to platform
- [ ] Redeploy app
- [ ] Test: Visit app and download video
- [ ] ✨ Works for everyone!

---

**That's it! Your users won't need to do anything.** 🎉
