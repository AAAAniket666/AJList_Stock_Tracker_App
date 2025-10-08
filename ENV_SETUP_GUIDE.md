# Environment Variables Setup Guide

This guide will help you set up all required environment variables for the Stock Tracker App.

## Quick Start

1. Copy `.env.example` to `.env`:
   ```powershell
   Copy-Item .env.example .env
   ```

2. Follow the instructions below to fill in each value.

---

## Required Environment Variables

### 1. FINNHUB API Key
**Purpose:** Fetch real-time stock market data, company news, and financial information.

**How to get it:**
1. Visit [Finnhub.io](https://finnhub.io/)
2. Sign up for a free account
3. Navigate to your dashboard
4. Copy your API key
5. Add to `.env`:
   ```
   NEXT_PUBLIC_FINNHUB_API_KEY=your_api_key_here
   ```

**Note:** The `NEXT_PUBLIC_` prefix exposes this to the browser. For server-only usage, use `FINNHUB_API_KEY` instead.

---

### 2. MongoDB Connection String
**Purpose:** Store user data, watchlists, and authentication information.

**How to get it:**
1. Visit [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account and cluster
3. Create a database user:
   - Go to "Database Access"
   - Add a new database user with a strong password
4. Whitelist your IP:
   - Go to "Network Access"
   - Add IP Address (or allow access from anywhere for testing: `0.0.0.0/0`)
5. Get connection string:
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user password
6. Add to `.env`:
   ```
   MONGODB_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/stocktracker?retryWrites=true&w=majority
   ```

---

### 3. Better Auth Secret
**Purpose:** Secure session tokens and authentication.

**How to generate it:**

Run this command in PowerShell:
```powershell
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Copy the output and add to `.env`:
```
BETTER_AUTH_SECRET=your_generated_secret_here
```

**Security:** Never share this secret or commit it to version control!

---

### 4. Gemini API Key
**Purpose:** Power AI features like stock analysis and news summarization.

**How to get it:**
1. Visit [Google AI Studio](https://aistudio.google.com/)
2. Sign in with your Google account
3. Click "Get API Key"
4. Create a new API key
5. Add to `.env`:
   ```
   GEMINI_API_KEY=your_gemini_api_key_here
   ```

**Alternative:** Use Google Cloud Console if you need more control or quota.

---

### 5. Nodemailer (Email Service)
**Purpose:** Send welcome emails and news summaries to users.

**Option 1: Gmail (for testing)**
1. Use a Gmail account (preferably a test account, not your main one)
2. Enable 2-Factor Authentication on your Google account
3. Generate an App Password:
   - Go to [Google Account Security](https://myaccount.google.com/security)
   - Select "2-Step Verification"
   - Scroll to "App passwords"
   - Generate a new app password for "Mail"
4. Add to `.env`:
   ```
   NODEMAILER_EMAIL=youremail@gmail.com
   NODEMAILER_PASSWORD=your_16_character_app_password
   ```

**Option 2: SMTP Provider (recommended for production)**
- [SendGrid](https://sendgrid.com/) - 100 emails/day free
- [Mailgun](https://www.mailgun.com/) - 5,000 emails/month free
- [Postmark](https://postmarkapp.com/) - 100 emails/month free

For SendGrid example:
```
NODEMAILER_EMAIL=apikey
NODEMAILER_PASSWORD=your_sendgrid_api_key
```
(You'll also need to modify `lib/nodemailer/index.ts` to use SendGrid's SMTP settings)

---

## Security Best Practices

### For Development
- ✅ Keep `.env` in `.gitignore` (already configured)
- ✅ Use `.env.example` for sharing configuration structure
- ✅ Never commit real credentials
- ✅ Use separate API keys for development and production

### For Production
- 🔐 Use environment variable services:
  - Vercel: Project Settings → Environment Variables
  - Railway: Variables tab
  - AWS: Secrets Manager or Parameter Store
- 🔐 Rotate API keys regularly
- 🔐 Use different MongoDB databases for dev/staging/production
- 🔐 Set `NODE_ENV=production`
- 🔐 Update `NEXT_PUBLIC_BASE_URL` and `BETTER_AUTH_URL` to your domain

---

## Verification Checklist

After setting up your `.env`, verify each service:

- [ ] **Finnhub:** Test by searching for a stock symbol
- [ ] **MongoDB:** Check the database connection in the app logs
- [ ] **Better Auth:** Try signing up a new user
- [ ] **Gemini:** Check AI-powered features load
- [ ] **Nodemailer:** Send a test welcome email

---

## Troubleshooting

### MongoDB Connection Issues
- Ensure your IP is whitelisted in Network Access
- Verify the username and password are correct
- Check that the database name in the URI exists

### Gmail App Password Not Working
- Ensure 2FA is enabled
- Generate a new app password if the old one doesn't work
- Check for spaces in the app password (remove them)

### Finnhub Rate Limits
- Free tier: 60 API calls/minute
- Upgrade to a paid plan if you hit limits

### Gemini API Errors
- Verify the API key is active in Google AI Studio
- Check quota limits in your Google Cloud project

---

## Need Help?

- Check the main [README.md](./README.md) for setup instructions
- Review the [Better Auth Documentation](https://better-auth.com/)
- Visit [Finnhub API Docs](https://finnhub.io/docs/api)

---

**Last Updated:** October 8, 2025
