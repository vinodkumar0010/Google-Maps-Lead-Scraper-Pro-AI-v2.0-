# 🚀 Hostinger Deployment Guide
## Google Maps Lead Scraper Pro AI

---

## YOUR KEYS GO IN ONE FILE: `.env`

You **never** share your keys with anyone. You fill them into a `.env` file on your own computer, build the project, and upload the output to Hostinger.

---

## Step 1: Download the Code

Download/clone this entire project folder to your computer.

---

## Step 2: Install Node.js

If you don't have Node.js installed:
- Go to https://nodejs.org
- Download LTS version (v20+)
- Install it
- Open Terminal / Command Prompt

---

## Step 3: Install Dependencies

Open Terminal in the project folder:

```bash
cd maps-scraper-pro
npm install
```

---

## Step 4: Create Your .env File

Copy the example file:

```bash
cp .env.example .env
```

Now open `.env` in any text editor (Notepad, VS Code) and fill in YOUR real values:

```env
# SUPABASE — get from https://supabase.com → Settings → API
VITE_SUPABASE_URL=https://abcdefgh.supabase.co
VITE_SUPABASE_ANON_KEY=

# PAYUMONEY — get from PayUMoney Dashboard → My Account
VITE_PAYU_MERCHANT_KEY=your_merchant_key_here
VITE_PAYU_MODE=test

# PRICES IN INR
VITE_PRICE_PRO=2399
VITE_PRICE_AGENCY=8199
VITE_PRICE_ENTERPRISE=24799

# EMAIL — get from https://resend.com → API Keys
VITE_EMAIL_FROM=noreply@masteredge.tech

# GOOGLE PLACES — your existing API key
VITE_GOOGLE_PLACES_KEY=AIzaSyxxxxxxxxxx

# YOUR DETAILS
VITE_ADMIN_EMAIL=owner@masteredge.tech
VITE_APP_URL=https://yourdomain.com
```

**Save the file.**

---

## Step 5: Build for Production

```bash
npm run build
```

This creates a `dist/` folder with your production files.

---

## Step 6: Upload to Hostinger

### Option A: Hostinger File Manager

1. Login to **Hostinger → hPanel**
2. Go to **File Manager**
3. Navigate to `public_html/` (or your domain's root folder)
4. **Delete** everything inside `public_html/` (backup first if needed)
5. **Upload** everything from your local `dist/` folder into `public_html/`
6. Your site should have these files:
   ```
   public_html/
   ├── index.html
   ├── assets/
   │   ├── index-xxxxx.js
   │   └── style-xxxxx.css
   └── masteredge-logo.png
   ```

### Option B: Hostinger FTP (FileZilla)

1. Hostinger → hPanel → **FTP Accounts** → Get FTP credentials
2. Open FileZilla → Connect using:
   - Host: your FTP hostname
   - Username: your FTP username
   - Password: your FTP password
   - Port: 21
3. Navigate to `public_html/`
4. Upload everything from local `dist/` folder

### Option C: Hostinger SSH + Git

1. Hostinger → hPanel → **SSH Access** → Enable SSH
2. SSH into your server:
   ```bash
   ssh u123456@your-server.hostinger.com
   ```
3. Clone your repo:
   ```bash
   cd public_html
   git clone https://github.com/YOUR-ORG/maps-scraper-pro.git .
   npm install
   cp .env.example .env
   nano .env  # fill in your keys
   npm run build
   # Move build files to root
   cp -r dist/* .
   ```

---

## Step 7: Configure .htaccess (IMPORTANT for React SPA)

Create a file called `.htaccess` in `public_html/` with this content:

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>

# Security headers
<IfModule mod_headers.c>
  Header set X-Content-Type-Options "nosniff"
  Header set X-Frame-Options "SAMEORIGIN"
  Header set X-XSS-Protection "1; mode=block"
  Header set Referrer-Policy "strict-origin-when-cross-origin"
</IfModule>

# Caching for static assets
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType text/css "access plus 1 year"
  ExpiresByType application/javascript "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/webp "access plus 1 year"
  ExpiresByType image/svg+xml "access plus 1 year"
</IfModule>

# Gzip compression
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html
  AddOutputFilterByType DEFLATE text/css
  AddOutputFilterByType DEFLATE application/javascript
  AddOutputFilterByType DEFLATE application/json
</IfModule>
```

---

## Step 8: Enable SSL (HTTPS)

1. Hostinger hPanel → **SSL** → Install free SSL certificate
2. Wait 5-10 minutes for it to activate
3. Your site is now at `https://yourdomain.com`

---

## Step 9: Verify Everything Works

Open `https://yourdomain.com` in your browser and check:

- [ ] Login page loads
- [ ] Can sign up with new account
- [ ] Can login with admin email + "Admin@123"
- [ ] Dashboard loads after login
- [ ] Scraper engine works
- [ ] All tabs show data
- [ ] No console errors (F12 → Console)

---

## 📁 File Structure on Hostinger

After uploading, your `public_html/` should look like:

```
public_html/
├── .htaccess          ← you create this (Step 7)
├── index.html         ← from dist/ folder
├── masteredge-logo.png
├── Google-Maps-Lead-Scraper-Pro-AI-Production-Guide.html
└── assets/
    ├── index-xxxxx.js
    └── style-xxxxx.css
```

---

## 🔄 How to Update the App Later

1. Make code changes on your computer
2. Run `npm run build`
3. Upload the new `dist/` folder contents to `public_html/`
4. Refresh your browser (Ctrl+Shift+R)

---

## ⚠️ Security Notes

- `.env` file stays on YOUR computer only — never upload it to Hostinger
- The build process bakes env values into the JS bundle at build time
- For production security, move API keys to Supabase Edge Functions
- Always use HTTPS (SSL enabled)

---

## 🆘 Troubleshooting

**Blank page after upload:**
→ Check that `.htaccess` file exists in `public_html/`
→ Check that `index.html` is in `public_html/` root (not inside a subfolder)

**Page loads but shows error:**
→ Open browser DevTools (F12) → Console tab → check for errors
→ Make sure you ran `npm run build` after filling `.env`

**Login doesn't work:**
→ Default admin: your VITE_ADMIN_EMAIL with password "Admin@123"
→ Clear browser localStorage and try again

**Google Places not working:**
→ Check your API key is correct in `.env`
→ Check Google Cloud Console → API key has "Places API (New)" enabled
→ Check API key restrictions allow your domain

---

## Need to Connect Supabase Later?

1. `npm install @supabase/supabase-js`
2. Fill `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in `.env`
3. Open `src/lib/supabase.ts` → Change `isSupabaseConfigured = true`
4. Rebuild: `npm run build`
5. Re-upload `dist/` to Hostinger
