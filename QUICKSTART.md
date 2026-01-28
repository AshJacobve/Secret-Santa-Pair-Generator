# 🎅 Secret Santa Generator - Quick Start Guide

## What You've Built

A fully functional Secret Santa website that:
- Takes participant names and Gmail addresses
- Creates random pairs (ensuring no one gets themselves)
- Sends email notifications to participants with their assignments
- Features a beautiful, modern UI with Tailwind CSS

## Files Created

### Components
- **`components/participant-form.tsx`** - Form to add participants with validation
- **`components/pairing-results.tsx`** - Display pairs and send emails

### Utilities
- **`lib/secret-santa.ts`** - Pairing algorithm (derangement algorithm)

### API
- **`app/api/send-emails/route.ts`** - Email sending endpoint

### Pages
- **`app/page.tsx`** - Main application page

## Quick Setup (3 Steps)

### Step 1: Install Dependencies
```bash
cd c:\code\secretsantA\secretsanta
npm install
```

### Step 2: (Optional) Configure Email

To enable email sending, set up Gmail App Password:

1. Go to [Google Account Security](https://myaccount.google.com/security)
2. Enable 2-Factor Authentication if not already enabled
3. Find "App passwords" and generate one for Mail/Windows
4. Create `.env.local` in the project root:
```
GMAIL_USERNAME=your-email@gmail.com
GMAIL_APP_PASSWORD=your-16-character-app-password
```

### Step 3: Run the App
```bash
npm run dev
```

Open `http://localhost:3000` in your browser!

## How to Use

1. **Add Participants**
   - Enter a name and Gmail address
   - Click "Add Participant" to add more people
   - Need at least 3 people minimum

2. **Create Pairs**
   - Once you have 3+ participants, click "Create Pairs"
   - The algorithm automatically pairs everyone (no one gets themselves!)

3. **Send Emails**
   - Review the pairs
   - Click "Send Emails to Participants"
   - Each person gets an email with WHO they're buying for (their recipient)

## Key Features Explained

### Pairing Algorithm
- Uses a **derangement algorithm** (mathematical term for "no one in original position")
- Guarantees no participant is paired with themselves
- Randomizes results each time you create pairs
- Falls back to rotation if random shuffling fails

### Email Functionality
- Emails are sent via Gmail SMTP (using Nodemailer)
- Each participant gets an HTML email with festive formatting
- Email contains only their assignment (not everyone's pairs)
- Works offline mode if credentials aren't configured

### Form Validation
- Name and email required for all participants
- Validates Gmail addresses (@gmail.com only)
- Checks for duplicate emails
- Minimum 3 participants required

## Component Architecture

```
Page (app/page.tsx)
├── ParticipantForm (components/participant-form.tsx)
│   └── Handles input and validation
└── PairingResults (components/pairing-results.tsx)
    └── Shows pairs and sends emails
    
API
└── /api/send-emails/route.ts
    └── Sends emails to participants
```

## Customization Ideas

### Change Email Template
Edit `app/api/send-emails/route.ts` (lines 50-80) to customize:
- Subject line
- Email body text
- HTML styling

### Change Minimum Participants
Edit `components/participant-form.tsx` (line 62):
```typescript
disabled={loading || participants.length < 3}  // Change 3 to any number
```

### Accept Different Email Providers
Edit `components/participant-form.tsx` (line 45):
```typescript
// Change from:
/^[^\s@]+@gmail\.com$/.test(p.email.trim())
// To:
/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(p.email.trim())  // Any email
```

## Troubleshooting

### Emails not sending?
- Check `.env.local` exists with correct credentials
- Verify 2FA is enabled on Gmail account
- Try generating a new App Password
- Check browser console (F12) for errors

### App won't start?
```bash
npm install
npm run dev
```

### Pairing errors?
- Ensure at least 3 participants
- Check all emails are unique
- Verify all names are filled in

### Styling issues?
- Clear browser cache (Ctrl+Shift+Del)
- Check if Tailwind CSS is loading (inspect page in DevTools)

## Next Steps / Enhancements

Want to add more features? Here are some ideas:

1. **Budget Limits**
   - Add budget input field
   - Show budget in email

2. **Gift Preferences**
   - Add notes field for each participant
   - Include preferences in recipient info

3. **Database Storage**
   - Save groups to database
   - Create multiple events
   - Track past events

4. **Admin Dashboard**
   - View all created groups
   - Edit/modify pairs
   - Resend emails

5. **Calendar Integration**
   - Set gift deadline
   - Send reminders
   - Track gift exchanges

## Important Notes

⚠️ **Security:**
- Never commit `.env.local` to Git (already in .gitignore)
- App Passwords are safer than account passwords
- Consider a dedicated Gmail account for the app
- In production, use OAuth2 instead of storing credentials

📧 **Email Limits:**
- Gmail allows ~500 emails/day from same account
- For larger groups, consider SendGrid/Mailgun

🔍 **Privacy:**
- No data is stored on any server
- Everything runs in browser and local API
- Emails are sent directly from configured Gmail account

## File Reference

| File | Purpose |
|------|---------|
| `app/page.tsx` | Main application interface |
| `components/participant-form.tsx` | Add/manage participants form |
| `components/pairing-results.tsx` | Show pairs and email button |
| `lib/secret-santa.ts` | Pairing algorithm logic |
| `app/api/send-emails/route.ts` | Email sending API |
| `.env.local` | Gmail credentials (create this) |

## Deployment

To deploy to production:

1. **Build the app:**
   ```bash
   npm run build
   ```

2. **Deploy to Vercel (recommended for Next.js):**
   - Push to GitHub
   - Connect to Vercel
   - Add `GMAIL_USERNAME` and `GMAIL_APP_PASSWORD` as environment variables

3. **Alternative hosting:**
   - Netlify
   - AWS
   - DigitalOcean
   - Any Node.js hosting

---

**You now have a fully functional Secret Santa generator! 🎁✨**

Questions? Check the main README.md for more detailed documentation.
