# 🧪 Testing Admin Email Setup

## ✅ Current Configuration

**Testing Email Active**:

- **Email**: `afaqahmadnaeem@gmail.com`
- **Location**: `.env.local` line 23

**Production Email** (Ready to use):

- **Email**: `mg@growth-onomics.com`
- **Location**: `.env.local` line 20 (commented out)

---

## 🚀 How to Test

### Step 1: Restart Dev Server

Environment variables are loaded on server start, so restart your dev server:

```bash
# If dev server is running, stop it (Ctrl+C)
# Then start again:
pnpm dev
```

### Step 2: Test the Admin Email Notification

**Option A - Quick Test Endpoint**:

```bash
curl -X POST http://localhost:3000/api/test-admin-email
```

**Option B - Register a New User**:

- Register through the app
- Check email at: `afaqahmadnaeem@gmail.com`
- Look for subject: `Backlinku: New User Registration - [UserName]`

---

## 🔄 Switching Between Test and Production

### To Switch to **Production Email** (mg@growth-onomics.com):

Edit `.env.local`:

```env
# Comment out test email
# ADMIN_EMAIL=afaqahmadnaeem@gmail.com

# Uncomment production email
ADMIN_EMAIL=mg@growth-onomics.com
```

### To Switch back to **Test Email** (afaqahmadnaeem@gmail.com):

Edit `.env.local`:

```env
# Comment out production email
# ADMIN_EMAIL=mg@growth-onomics.com

# Uncomment test email
ADMIN_EMAIL=afaqahmadnaeem@gmail.com
```

**Important**: After changing `.env.local`, **always restart your dev server**!

---

## 📧 What You'll Receive

**Email Details**:

- **To**: afaqahmadnaeem@gmail.com (during testing)
- **From**: noreply@app.backlinku.com
- **Subject**: `Backlinku: New User Registration - [UserName]`

**Email Content**:

- User's full name
- User's email address
- Registration date and time
- Professional formatted template

---

## ✅ Quick Test Checklist

- [ ] Updated `.env.local` with test email ✅
- [ ] Restart dev server to load new config
- [ ] Send test email: `curl -X POST http://localhost:3000/api/test-admin-email`
- [ ] Check terminal for: `✅ Admin notification sent successfully`
- [ ] Check inbox: `afaqahmadnaeem@gmail.com`
- [ ] Verify email content

---

## 🎯 Production Deployment

**For Production**:

1. Make sure in production environment:
   - `ADMIN_EMAIL=mg@growth-onomics.com`
2. No need to change any code
3. The code will work the same way

---

## 📝 Notes

- **Test Email**: For local testing only
- **Production Email**: mg@growth-onomics.com (kept ready)
- **No Code Changes Needed**: Only environment variable changes
- **Email Service Works the Same**: Just different recipient
