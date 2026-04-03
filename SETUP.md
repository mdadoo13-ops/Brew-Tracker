# ☕ Brew Tracker — Setup & Deploy Guide

## What's in this folder

| File | What it does |
|---|---|
| `index.html` | The full app |
| `manifest.json` | Makes it installable on phones |
| `sw.js` | Lets it work offline |
| `icon-192.png` | App icon (home screen) |
| `icon-512.png` | App icon (splash screen) |

All 5 files must stay in the same folder.

---

## STEP 1 — Set up Firebase (free, ~5 min)

1. Go to https://console.firebase.google.com
2. Click **"Add project"** → name it anything (e.g. "brew-tracker") → **disable** Google Analytics → Create
3. Inside the project, go to **Build → Firestore Database**
4. Click **"Create database"** → choose **`europe-west`** region → **"Start in test mode"** → Enable
5. Go to **Project Settings** (⚙️ gear icon, top left)
6. Scroll to **"Your apps"** → click the `</>` web icon → name it anything → **Register app**
7. Copy the `firebaseConfig` object — looks like:
```json
{
  "apiKey": "AIza...",
  "authDomain": "your-app.firebaseapp.com",
  "projectId": "your-app",
  "storageBucket": "your-app.appspot.com",
  "messagingSenderId": "123456789",
  "appId": "1:123:web:abc"
}
```

---

## STEP 2 — Deploy to Netlify (free, ~2 min)

1. Go to https://app.netlify.com and sign up (free)
2. On the dashboard, find the **"Deploy manually"** box at the bottom
3. **Drag the entire `brew-tracker-pwa` folder** onto that box
4. Netlify gives you a URL like `https://random-name.netlify.app`
5. Done — your app is live!

**To get a nicer URL:**
- In Netlify → Site settings → Site name → Change to e.g. `saddle-creek-brew`
- Your URL becomes `https://saddle-creek-brew.netlify.app`

---

## STEP 3 — Install on your phone

### Android
1. Open the URL in **Chrome**
2. A banner appears: "Add Brew Tracker to home screen" — tap it
3. Or: tap the ⋮ menu → "Add to Home screen"

### iPhone (iOS)
1. Open the URL in **Safari** (must be Safari, not Chrome)
2. Tap the **Share** button (box with arrow)
3. Tap **"Add to Home Screen"**
4. Tap **Add**

### For staff
Just send them the Netlify URL via WhatsApp. They follow the same install steps on their phones.

---

## STEP 4 — Enter your Firebase config

When you first open the app, it shows a setup screen asking for your Firebase config.
Paste the JSON from Step 1 and tap **"Connect & Open App"**.

The config is saved on each device — staff only need to do this once.

---

## Updating the app

When you want new features added:
1. Tell me what you want
2. I'll give you a new `index.html`
3. Replace the file in your folder
4. Drag the folder to Netlify again
5. Everyone's app updates within seconds — no App Store needed

---

## Security note (important)

Firebase "test mode" allows open read/write for 30 days. Before that expires:

1. Go to Firebase Console → Firestore → **Rules**
2. Replace the rules with:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```
This keeps it open for your team. If you want password protection later, I can add a simple PIN login.

---

## Troubleshooting

**"Could not connect"** — Double-check you copied the full firebaseConfig JSON, including all 6 fields.

**App not updating after Netlify deploy** — Hard refresh: hold Shift + tap reload, or clear browser cache.

**iOS install prompt not showing** — Must use Safari. Chrome on iPhone doesn't support PWA install.

**Data not syncing between phones** — Make sure all devices entered the same Firebase config (same project ID).
