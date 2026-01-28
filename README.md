# Secret Santa Generator 🎅
<img width="2851" height="1499" alt="image" src="https://github.com/user-attachments/assets/02eb8cb8-bf6b-4b15-b8d3-737e7f9cee28" />

A modern, easy-to-use web application that creates Secret Santa pairs and sends notifications to participants via email.

## Features

✨ **Key Features:**
- 📝 Simple form to add participant names and Gmail addresses
- 🎲 Intelligent pairing algorithm that ensures no one gets themselves
- 📧 Automated email notifications to participants
- 🎨 Beautiful, modern UI with Tailwind CSS
- 📱 Fully responsive design
- 🌓 Dark/Light mode support

## How It Works

1. **Add Participants**: Enter the names and Gmail addresses of all participants
2. **Create Pairs**: The app generates random Secret Santa pairs, ensuring no one is paired with themselves
3. **Send Notifications**: Emails are sent to each participant informing them who they're buying a gift for
4. **Keep It Secret**: The recipient only knows their own assignment, not who is buying for them

## Setup Instructions

### Prerequisites

- Node.js 18+ 
- npm or yarn
- Gmail account (for sending emails)

### Installation

1. **Navigate to the project directory:**
   ```bash
   cd secretsanta
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure Email (Optional)**

   To enable email sending, you need to set up Gmail credentials:

   #### Using Gmail App Password (Recommended):
   
   1. Enable 2-Factor Authentication on your Google Account
   2. Go to [Google Account Security](https://myaccount.google.com/security)
   3. Look for "App passwords" and create one for "Mail" and "Windows"
   4. Create a `.env.local` file in the project root:
      ```
      GMAIL_USERNAME=your-email@gmail.com
      GMAIL_APP_PASSWORD=your-app-password
      ```

   **Note:** Without these credentials, the app will still work but won't send emails. It will show a message that email service is not configured.

### Running the Application

#### Development Mode:
```bash
npm run dev
```

The app will be available at `http://localhost:3000`

#### Production Build:
```bash
npm run build
npm start
```

## Usage Guide

### Adding Participants

1. Click "Add Participant" to add more people to your Secret Santa group
2. Enter each person's name and Gmail address
3. A minimum of 3 participants is required
4. You can remove participants by clicking the trash icon

### Creating Pairs

1. After adding all participants, click "Create Pairs"
2. The algorithm will create random pairs ensuring:
   - No one is paired with themselves
   - All pairs are valid

### Sending Notifications

1. Review the created pairs to ensure they look correct or ignore if you are a participant
2. Click "Send Emails to Participants"
3. Each participant will receive an email with:
   - A festive Secret Santa notification
   - The name of the person they're buying a gift for
   - A reminder to keep it secret

## Technical Stack

- **Framework**: Next.js 15 (React 19)
- **Language**: TypeScript
- **Styling**: Tailwind CSS 4
- **Email**: Nodemailer
- **Icons**: Lucide React
- **UI Components**: Custom Radix UI-based components

## Project Structure

```
secretsanta/
├── app/
│   ├── api/
│   │   └── send-emails/
│   │       └── route.ts          # Email API endpoint
│   ├── globals.css               # Global styles
│   ├── layout.tsx                # Root layout
│   └── page.tsx                  # Main page
├── components/
│   ├── participant-form.tsx      # Participant input form
│   ├── pairing-results.tsx       # Results display
│   ├── header.tsx                # Header component
│   ├── hero-section.tsx          # Hero component
│   ├── logo.tsx                  # Logo component
│   └── ui/                       # UI components
│       ├── button.tsx
│       ├── text-effect.tsx
│       └── animated-group.tsx
├── lib/
│   ├── utils.ts                  # Utility functions
│   └── secret-santa.ts           # Pairing algorithm
└── public/                       # Static assets
```

## Pairing Algorithm

The app uses a **derangement algorithm** to create valid pairs:

- A derangement is a permutation where no element appears in its original position
- This ensures no participant is paired with themselves
- The algorithm uses random shuffling with validation
- Falls back to a guaranteed rotation algorithm if needed

## Environment Variables

Create a `.env.local` file for email configuration:

```env
# Gmail configuration (optional)
GMAIL_USERNAME=your-email@gmail.com
GMAIL_APP_PASSWORD=your-app-password
```

### Gmail API (OAuth2) — recommended for production

If you'd like to use the Gmail API (recommended instead of App Passwords), add these variables to `.env.local` and install the `googleapis` package:

```env
# Google OAuth2 credentials
GOOGLE_CLIENT_ID=your-google-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_REFRESH_TOKEN=your-oauth2-refresh-token
SENDER_EMAIL=your-sender-email@example.com
SENDER_NAME=Optional Sender Name
```

Install the client library:

```bash
npm install googleapis
```

Notes:
- To get a refresh token you can run a one-time OAuth2 flow with your Google Cloud project's OAuth2 client credentials and obtain the refresh token for the sending account.
- For GSuite/Workspace accounts you can also use domain-wide delegation.
- The app's API route will automatically use Gmail API when these env vars are present. If they are not configured, the API will skip sending and still return the created pairs.

## Email Configuration

### Using Gmail:

The app uses `nodemailer` with Gmail's SMTP server. To send emails:

1. Enable 2-Factor Authentication on your Google Account
2. Generate an App Password:
   - Go to [Google Account Security](https://myaccount.google.com/security)
   - Select "App passwords"
   - Choose "Mail" and "Windows"
   - Copy the generated password
3. Add to `.env.local`:
   ```
   GMAIL_USERNAME=your-email@gmail.com
   GMAIL_APP_PASSWORD=your-generated-app-password
   ```

### Alternative Email Services:

You can modify `/app/api/send-emails/route.ts` to use:
- SendGrid
- Mailgun
- AWS SES
- Any other email service


## Customization

### Changing Email Template

Edit `/app/api/send-emails/route.ts` to customize:
- Email subject line
- Email body content
- HTML styling

### Modifying Validation

Edit `/components/participant-form.tsx` to:
- Change minimum participant count
- Accept different email providers
- Add additional fields

### Styling

All styling uses Tailwind CSS classes. Modify:
- Component classes directly
- Theme colors in `/app/globals.css`
- CSS variables in the theme section

## Troubleshooting

### Emails not sending?

1. Check `.env.local` exists with correct credentials
2. Verify Gmail account has App Passwords enabled
3. Check browser console for error messages
4. Ensure all participants have valid Gmail addresses

### Pairing issues?

1. Ensure at least 3 participants
2. Check all emails are unique
3. Verify no participant emails are duplicated

### UI issues?

1. Clear browser cache
2. Run `npm run build` to check for build errors
3. Check console for JavaScript errors

## Future Enhancements

Possible improvements:
- [ ] Budget limits per participant
- [ ] Gift preference notes
- [ ] Integration with gift registries
- [ ] Multiple language support
- [ ] Calendar/deadline tracking
- [ ] Photo gallery sharing
- [ ] Database storage for groups
- [ ] Admin dashboard
- [ ] SMS notifications

## Contributing

Feel free to submit issues and enhancement requests!

## License

MIT License - feel free to use this in your projects

## Support

For issues or questions, please open an issue or contact support.

---

**Enjoy creating magical Secret Santa moments! 🎁✨**

