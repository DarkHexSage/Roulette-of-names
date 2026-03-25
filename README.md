# 🎰 Roulette Giveaway — Setup Guide

A real-time giveaway roulette wheel hosted on **GitHub Pages** with **Firebase Firestore** as the backend.

---

## How it works

| Who | What they do |
|-----|-------------|
| **Host** | Opens the app → creates a session → gets a shareable link |
| **Participants** | Open the shared link → register with their name → wait |
| **Host** | Watches names appear live → hits SPIN → wheel picks the winner |

Each giveaway is a fresh session. You can run as many as you want.

---

## Step 1 — Create a Firebase project

1. Go to **https://console.firebase.google.com**
2. Click **"Add project"** → name it (e.g. `my-roulette`) → click through the setup
3. Once created, click **"Firestore Database"** in the left sidebar
4. Click **"Create database"** → choose **Production mode** → pick any region → Done

### Set Firestore security rules

In the Firestore console → **Rules** tab, paste this and click **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /sessions/{sessionId} {
      allow read, write: if true;
      match /participants/{participantId} {
        allow read, write: if true;
      }
    }
  }
}
```

> ⚠️ This is open access — fine for a quick giveaway. For production, add authentication.

---

## Step 2 — Get your Firebase config

1. In Firebase Console → click the **⚙️ gear icon** → **Project settings**
2. Scroll down to **"Your apps"** → click **"</> Web"** to add a web app
3. Give it a name (e.g. `roulette-web`) → click **Register app**
4. Copy the `firebaseConfig` object — it looks like this:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "my-roulette.firebaseapp.com",
  projectId: "my-roulette",
  storageBucket: "my-roulette.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

---

## Step 3 — Paste the config into index.html

Open `index.html` and find this section near the bottom:

```js
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT_ID.firebaseapp.com",
  projectId:         "YOUR_PROJECT_ID",
  ...
};
```

Replace it with your actual values from Step 2.

---

## Step 4 — Deploy to GitHub Pages

```bash
# 1. Create a new GitHub repository (public)
#    e.g. https://github.com/youruser/roulette

# 2. Push your files
git init
git add index.html README.md
git commit -m "Initial deploy"
git branch -M main
git remote add origin https://github.com/youruser/roulette.git
git push -u origin main

# 3. Enable GitHub Pages:
#    GitHub repo → Settings → Pages → Source: "main" branch, / (root) → Save

# Your app will be live at:
# https://youruser.github.io/roulette/
```

---

## How to run a giveaway

1. Open **`https://youruser.github.io/roulette/`**
2. Enter your name → click **"Create New Giveaway"**
3. You'll be taken to the **host view** — copy the participant link shown at the top
4. Share that link with your audience (stream chat, Discord, Twitter, etc.)
5. Watch people register in real time on the left panel
6. When ready, hit **SPIN!** — the wheel picks a winner automatically
7. The winner is saved to history — export with **"Export CSV"** anytime
8. Click **"Reset Draw"** to clear participants and start a new round (history stays)

---

## Features

- ✅ Classic spinning wheel with smooth animation
- ✅ Real-time participant list (updates instantly for everyone)
- ✅ Duplicate name prevention
- ✅ Winner history with CSV export
- ✅ Session-based (each giveaway gets a unique link)
- ✅ Participants see if they won automatically
- ✅ Mobile friendly

---

## File structure

```
/
├── index.html    ← The entire app (single file)
└── README.md     ← This guide
```

No build tools, no npm, no dependencies beyond Firebase CDN and Google Fonts.
