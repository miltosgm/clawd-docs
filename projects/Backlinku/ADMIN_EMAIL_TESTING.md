# Admin Email Notification Testing Guide

## ✅ What I've Verified

### 1. **Configuration Check**

- **ADMIN_EMAIL**: `mg@growth-onomics.com` ✅ (configured in `.env.local`)
- **FROM_EMAIL**: `noreply@app.backlinku.com` ✅ (configured)
- **RESEND_API_KEY**: ✅ Set

### 2. **Code Implementation**

The admin notification email functionality is implemented in:

```149:180:app/api/register/route.ts
	// Send admin notification asynchronously (non-blocking)
	// Fire and forget - don't block user registration
	void (async () => {
		try {
			const emailService = EmailService.getInstance();
			console.log('📧 Sending admin notification for new user:', email);

			const result = await emailService.sendAdminNotification({
				adminName: 'Admin',
				userName: `${firstName} ${lastName}`,
				userEmail: email,
				registrationDate: new Date().toLocaleString('en-US', {
					year: 'numeric',
					month: 'long',
					day: 'numeric',
					hour: '2-digit',
					minute: '2-digit',
				}),
			});

			if (result.success) {
				console.log(
					'✅ Admin notification sent successfully. Message ID:',
					result.messageId
				);
			} else {
				console.error('❌ Failed to send admin notification:', result.error);
			}
		} catch (error) {
			console.error('❌ Error sending admin notification:', error);
		}
	})();
```

### 3. **Email Service**

The email service is configured at:

- **File**: `lib/emailService.ts` (lines 64-94)
- **Template**: `components/emails/NewUserRegistrationEmail.tsx`

---

## 🧪 How to Test

### Method 1: Test via API Endpoint (Recommended)

I've created a test API endpoint for you:

**Step 1**: Start the dev server

```bash
pnpm dev
```

**Step 2**: Check configuration

```bash
curl http://localhost:3000/api/test-admin-email
```

**Step 3**: Send test email

```bash
curl -X POST http://localhost:3000/api/test-admin-email
```

This will:

- ✅ Send a test admin notification email
- ✅ Show success/failure status
- ✅ Log to console

### Method 2: Test via New User Registration

**Step 1**: Register a new user through the app

- Go to registration page
- Fill in new user details
- Submit registration

**Step 2**: Check logs in terminal

- Look for: `📧 Sending admin notification for new user:`
- Look for: `✅ Admin notification sent successfully. Message ID:`

**Step 3**: Check admin email inbox

- Open `mg@growth-onomics.com`
- Look for email with subject: `Backlinku: New User Registration - [UserName]`

---

## 📧 What the Admin Will Receive

**Email Details**:

- **To**: mg@growth-onomics.com
- **Subject**: Backlinku: New User Registration - [UserName]
- **From**: noreply@app.backlinku.com

**Email Content**:

- User's full name
- User's email address
- Registration date and time
- Professional email template with user details

---

## 🐛 Troubleshooting

### Issue: Email not received

**Check 1**: Environment Variables

```bash
# Make sure these are set in .env.local
ADMIN_EMAIL=mg@growth-onomics.com
RESEND_API_KEY=re_65daQ9ja_DfXZgtvYeZn4DrLKvckRECHy
FROM_EMAIL=noreply@app.backlinku.com
```

**Check 2**: Resend API Key validity

- Verify your Resend account is active
- Check if the API key is valid
- Ensure you have credits in Resend

**Check 3**: Server logs

- Look for error messages in terminal
- Check for: `❌ Failed to send admin notification`
- Check for: `❌ Error sending admin notification`

**Check 4**: Spam folder

- Check spam/junk folder in mg@growth-onomics.com
- Email might be filtered as spam

---

## ✨ Recent Improvements Made

1. **Better Error Handling**: Changed from `setTimeout` to `void (async () => {...})()` pattern for proper async execution
2. **Enhanced Logging**: Added console logs to track email sending status
3. **Test Endpoint**: Created `/api/test-admin-email` for easy testing
4. **Proper Error Display**: Shows message ID on success and error details on failure

---

## 🎯 Quick Verification Checklist

- [ ] Start dev server (`pnpm dev`)
- [ ] Test configuration: `curl http://localhost:3000/api/test-admin-email`
- [ ] Send test email: `curl -X POST http://localhost:3000/api/test-admin-email`
- [ ] Check terminal for success message
- [ ] Check email inbox: mg@growth-onomics.com
- [ ] Verify email content and formatting
- [ ] Test with actual user registration
- [ ] Confirm email is received promptly

---

## 📝 Notes

- The admin notification is sent **asynchronously** (doesn't block user registration)
- Even if email fails, user registration still succeeds
- All email sending attempts are logged to console
- The email uses a professional template with user details
