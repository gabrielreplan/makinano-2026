# NLE 2026 Reviewer Tracker

A Progressive Web App (PWA) to track your Philippine Nursing Licensure Exam 2026 review progress.

## Features
- ✅ All 5 Nursing Practice subjects with topics from the official PRBON TOS
- 📊 Progress rings, bars, and statistics
- 📝 PNLE Score Tracker — Diagnostic / Pre-test / Post-test / Active Recall scores per subject, plus a 12-week Comprehensive Exam tracker, all with auto-computed percentages and averages
- 🔍 Search topics per subject
- ✓ Check All / Uncheck All per subject
- 💾 Progress saved locally (persists between sessions, and survives app updates)
- 🔄 Auto-updates in the background — when a new version is ready you'll see an "Update available" banner; tapping it refreshes the app without touching your saved data
- 📱 Installable on iPad, iPhone, Android (works offline!)
- 🔐 **Google Sign-In + paywall** — visitors sign in with Google, then see a "payment pending" screen until you manually approve them. Once approved, they're in for good (no re-approval needed, unless you revoke it).

---

## 🔐 Setting up Sign-In & the Paywall (Firebase — free)

This uses **Firebase**, Google's free backend service (Spark/free plan — no cost for the volume a tracker like this will see). It gives you real Google Sign-In and a place to approve/reject each buyer.

### Step A: Create a Firebase project
1. Go to https://console.firebase.google.com → **Add project** → name it anything (e.g. `nle-tracker`) → finish the wizard (Google Analytics is optional, you can skip it).

### Step B: Turn on Google Sign-In
1. In the left sidebar: **Build → Authentication → Get started**
2. Under **Sign-in method**, enable **Google** → set a support email → **Save**.

### Step C: Turn on Firestore (the database that stores who's approved)
1. Left sidebar: **Build → Firestore Database → Create database**
2. Choose **Start in production mode** → pick any region close to you → **Enable**.
3. Go to the **Rules** tab and replace the rules with this, then **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{userId} {
         allow read: if request.auth != null && request.auth.uid == userId;
         allow create: if request.auth != null && request.auth.uid == userId;
         allow update: if false; // only you can approve, via the Firebase Console
       }
     }
   }
   ```
   This means: a signed-in user can only see/create *their own* record, and can never set their own `approved` field to `true` — only you can do that, from the console.

### Step D: Get your config keys and paste them into `index.html`
1. Left sidebar: click the **⚙️ gear → Project settings**
2. Scroll to **Your apps → Web (</> icon)** → register an app (any nickname) → it shows a `firebaseConfig` object.
3. Open `index.html`, find this block near the top of the `<body>` (search for `YOUR_API_KEY`), and replace it with your real values:
   ```js
   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT.firebaseapp.com",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_PROJECT.appspot.com",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```

### Step E: Authorize your GitHub Pages domain
1. **Authentication → Settings → Authorized domains → Add domain**
2. Add `YOUR-USERNAME.github.io` (the domain your site will live on — see hosting steps below). Without this, Google Sign-In will fail with an "unauthorized domain" error.

### Step F: Set your price and payment details
Still in `index.html`, search for `[set your price]` and `[GCash / bank name]` (they appear twice each) and fill in your real price and payment info (GCash number, bank details, etc.), and `[your Messenger/FB name or contact]` with how buyers should reach you.

---

## ✅ Approving a buyer after payment

1. Buyer opens your site → signs in with Google → sees a **"Almost there!"** pending screen with their email and a User ID, and instructions to pay you and message you.
2. Once they pay and message you, go to **Firebase Console → Firestore Database → `users` collection**.
3. Find the document matching their email (or the User ID they sent you).
4. Click into it → edit the `approved` field → change it from `false` to `true` (boolean) → Save.
5. Their app unlocks **automatically within seconds** — they don't need to refresh or reinstall anything.

To revoke someone's access later, just flip `approved` back to `false` the same way.

> 💡 Tip: You can add extra fields to their document too (e.g. `paidAmount`, `notes`) — Firestore doesn't mind extra fields, and they won't affect the app.

## 🌐 FREE Hosting on GitHub Pages (Step-by-Step)

> Do the Firebase setup above **first** (at least Steps A–D, filling in your real `firebaseConfig`), then upload the edited `index.html` in Step 3 below.

### Step 1: Create a GitHub account
Go to https://github.com and sign up (free).

### Step 2: Create a new repository
1. Click the **+** button → **New repository**
2. Name it: `nle-tracker` (or anything you like)
3. Set to **Public**
4. Click **Create repository**

### Step 3: Upload the files
1. Click **uploading an existing file** on the repo page
2. Drag and drop ALL these files:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icon-192.png`
   - `icon-512.png`
3. Click **Commit changes**

> **Updating later?** Whenever you replace these files with a newer version, just upload and commit them the same way — the app will detect the new version and show an "Update available" banner to users automatically. No need to tell users to reinstall; their saved progress stays intact.

### Step 4: Enable GitHub Pages
1. Go to **Settings** tab of your repo
2. Scroll to **Pages** section
3. Under **Source**, select **Deploy from a branch**
4. Choose **main** branch and **/ (root)** folder
5. Click **Save**
6. Wait ~1 minute, then your site will be live at:
   `https://YOUR-USERNAME.github.io/nle-tracker/`

## 📱 Install on iPad / iPhone
1. Open the link in **Safari** (not Chrome on iOS)
2. Tap the **Share** button (box with arrow)
3. Scroll and tap **Add to Home Screen**
4. Tap **Add**
5. The app icon will appear on your home screen!

## 📱 Install on Android
1. Open the link in **Chrome**
2. Tap the **3-dot menu**
3. Tap **Add to Home Screen** or **Install App**
4. Tap **Install**

## 📁 Files
- `index.html` — Main app
- `manifest.json` — PWA manifest (makes it installable)
- `sw.js` — Service worker (enables offline use)
- `icon-192.png` — App icon (small)
- `icon-512.png` — App icon (large)
- `README.md` — This file

## Subjects Covered
| Subject | Description |
|---------|-------------|
| NP I | Community Health Nursing |
| NP II | Maternal, Child & Adolescent Nursing |
| NP III | Surgery, Oxygenation, F&E, Immunology |
| NP IV | GI, Endocrine, Neuro, MSK & Senses |
| NP V | Psychiatric Nursing & Emergency Care |
