# Secret Santa App Architecture

## Data Flow

```
User Interface
    ↓
ParticipantForm Component
    ├─ Collects names & emails
    ├─ Validates input
    └─ Calls createSecretSantaPairs()
        ↓
    Pairing Algorithm (lib/secret-santa.ts)
    ├─ Creates derangement
    ├─ Ensures no self-pairs
    └─ Returns Pair[] array
        ↓
    PairingResults Component
    ├─ Displays pairs
    └─ Triggers email sending
        ↓
    Send Emails Button
        ↓
    API Route (/api/send-emails)
    ├─ Receives pairs
    ├─ Creates emails
    └─ Sends via Gmail SMTP
        ↓
    Participants Receive Emails 📧
```

## Component Hierarchy

```
Home Page (app/page.tsx)
│
├─── Header Section
│    └─ Logo + Title
│
└─── Main Content Container
     │
     ├─── ParticipantForm Component
     │    ├─ Name Input
     │    ├─ Email Input
     │    ├─ Add/Remove Buttons
     │    └─ Create Pairs Button
     │
     └─── PairingResults Component
          ├─ Pair Display Cards
          ├─ Send Emails Button
          └─ Start Over Button
```

## State Management

```
app/page.tsx (Main State)
│
├─ pairs: Pair[] | null
│  └─ Stores created pairs
│
├─ participants: Participant[] | null
│  └─ Stores submitted participants
│
└─ loading: boolean
   └─ Tracks loading state
```

## API Structure

```
POST /api/send-emails
Input:
{
  "pairs": [
    {
      "giver": { "id", "name", "email" },
      "receiver": { "id", "name", "email" }
    }
  ]
}

Output:
{
  "success": true,
  "message": "Successfully sent 5 emails"
}
```

## Email Flow

```
API Endpoint receives pairs
        ↓
Create Nodemailer transporter
        ↓
For each pair:
  ├─ Create email content
  │  ├─ Subject: "Your Secret Santa Assignment"
  │  ├─ To: giver's email
  │  ├─ Body: "You're buying for: [receiver name]"
  │  └─ Format: HTML + Plain text
  │
  └─ Send via Gmail SMTP
        ↓
Track success/failure
        ↓
Return results to frontend
```

## Algorithm: Derangement

```
Input: [Person A, Person B, Person C, Person D]

Goal: Create pairs where:
- A doesn't gift to A
- B doesn't gift to B
- C doesn't gift to C
- D doesn't gift to D

Method 1: Random Shuffle (tries up to 100 times)
├─ Shuffle list of recipients
├─ Check if valid (no person = their own recipient)
└─ If valid, return; else retry

Method 2: Guaranteed Rotation (fallback)
├─ Rotate list by 1 position
├─ Person at index i receives from person at index (i+1) % n
└─ Guaranteed to be valid
```

## Pairing Example

```
Input Participants:
- Alice (alice@gmail.com)
- Bob (bob@gmail.com)
- Carol (carol@gmail.com)
- Dave (dave@gmail.com)

Pairing Algorithm Result:
Alice → buys for Carol
Bob → buys for Dave
Carol → buys for Bob
Dave → buys for Alice

Emails Sent:
alice@gmail.com: "You're buying for: Carol"
bob@gmail.com: "You're buying for: Dave"
carol@gmail.com: "You're buying for: Bob"
dave@gmail.com: "You're buying for: Alice"
```

## Technology Stack

```
Frontend
├─ React 19
├─ TypeScript
├─ Tailwind CSS 4
└─ Lucide Icons

Backend
├─ Next.js 15
├─ Node.js
└─ Nodemailer

Data
└─ In-memory (no database)
```

## Security Flow

```
User Input
    ↓
Client-side Validation
├─ Name not empty
├─ Email format check (@gmail.com)
└─ No duplicate emails
    ↓
Server-side Validation (API)
├─ Re-validate input
├─ Check credentials exist
└─ Send emails
    ↓
Gmail SMTP
├─ Authenticate with App Password
└─ Send emails securely
```

## File Dependencies

```
app/page.tsx
├─ components/participant-form.tsx
│  └─ lib/secret-santa.ts
├─ components/pairing-results.tsx
│  └─ /api/send-emails (fetch call)
└─ components/logo.tsx

app/api/send-emails/route.ts
└─ nodemailer (external package)

lib/secret-santa.ts
└─ (no dependencies)

components/participant-form.tsx
├─ components/ui/button.tsx
├─ lucide-react (icons)
└─ lib/secret-santa.ts
```

## Browser to Server Communication

```
Browser                          Server
   │                               │
   ├─ User fills form              │
   ├─ Click "Create Pairs"         │
   │                               │
   ├─ Call createSecretSantaPairs()│
   │ (in memory, no server call)   │
   │                               │
   ├─ Display results              │
   │                               │
   ├─ Click "Send Emails"          │
   │ POST /api/send-emails ───────>│
   │                    (pairs data)│
   │                               │
   │                 Process pairs │
   │        Create email content  │
   │      Send via Gmail SMTP     │
   │                               │
   │ <───── JSON response         │
   │        (success/error)        │
   │                               │
   └─ Show confirmation            │
```

## State Transitions

```
Initial State
    ↓ Add participants
ParticipantForm Active
    ├─ can add more
    ├─ can remove
    └─ can create pairs (if 3+)
         ↓
    Generate Pairs
         ↓
PairingResults Active
    ├─ can send emails
    ├─ can start over
         ↓
    Send Emails
         ↓
    Results Displayed
    (emails sent status)
         ↓
    Click "Start Over"
         ↓
Back to ParticipantForm Active
```

---

This architecture ensures:
✅ Clean separation of concerns
✅ Reusable components
✅ Secure email handling
✅ User-friendly flow
✅ Error handling at each step
