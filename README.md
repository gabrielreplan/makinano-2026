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
- 🤖💳 **AI Study Assistant is a separate paid add-on** — the base tracker (progress, scores, comp-exam tracker, cloud sync) is gated by one payment/approval; the AI Chat/Explain/Quiz/Advice assistant is gated by a *second, independent* payment/approval. Someone can buy just the tracker, or the tracker + AI — you set both prices and approve each separately.
- ☁️ **Automatic cloud sync** — once approved, progress/scores/comp-exam data/exam date are mirrored to that person's own Firestore document. Sign in with the same Google account on another phone/tablet/browser and it pulls the same data down automatically — no manual export/import needed. A small "☁ Synced" indicator in the top bar shows sync status.

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
         allow update: if request.auth != null && request.auth.uid == userId
                       && request.resource.data.approved == resource.data.approved
                       && request.resource.data.aiApproved == resource.data.aiApproved;
       }
     }
   }
   ```
   This means: a signed-in user can only see/create *their own* record, and can update their own `sync` data (that's what powers cloud sync, below) — but `allow update` requires **both** the `approved` field *and* the `aiApproved` field to stay exactly as they already were, so a user can never flip either of them from `false` to `true` themselves. Only you can do that, from the console — and you do it independently for each: someone can be `approved: true` (tracker) but still `aiApproved: false` (no AI yet) until they pay for the add-on too.

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
Still in `index.html`, there are now **two separate paywalls**:

1. **Base tracker paywall** (sign-in + pending screens) — payment name/number/Messenger are already filled in with example values; search for `₱ 200` to change the price, and update the name/number/Messenger link near `gate-instructions` / `gate-msg-link` if they're not yours.
2. **AI Study Assistant add-on paywall** (shown when someone taps the 🤖 button without having paid for AI) — search for `[set your AI price]` (appears twice) and set your AI price, and `[GCash / bank name]` / `[your Messenger/FB name or contact]` (inside the `id="ai-paywall-overlay"` block) to fill in your real payment details. It reuses the same Messenger link as the base paywall by default — change the `href` next to `id="ai-paywall-msg-link"` if you want a different contact for AI upgrade requests.

Want the base tracker to be free and only charge for the AI add-on? Just change the `₱ 200` price text to "Free" (or remove the price line) and set `approved: true` automatically for everyone — the simplest way is to change the default in the `userRef.set({...})` call (search for `approved: false,`) to `approved: true,` so new sign-ins skip the base paywall entirely and land straight on the pending-AI screen only when they tap the AI button.

---

## ☁️ How Cloud Sync works

Once someone is approved, their progress/scores/comp-exam data/exam date are:
1. Kept in `localStorage` on their device (instant, works offline).
2. Mirrored (debounced ~1s after each change) into their own `users/{uid}` Firestore document, under a `sync` field.

When they sign in with the *same Google account* on another device, the app compares timestamps and — if the cloud copy is newer — pulls it down and reloads automatically. No button to press.

**Good to know:**
- This is "last write wins," not a merge. If you edit on two devices at the same moment while both are online, whichever save reaches the server last overwrites the other. For one person switching between their own phone/tablet, this is a non-issue in practice.
- Sync is scoped to each Google account's own Firestore document — one buyer's data never touches another's.
- If two *different* people ever sign into the *same* browser/device one after another (a shared tablet, say), the app doesn't currently namespace `localStorage` by account, so there's a brief window where the second person could see the first person's cached local data until the cloud sync for their own account kicks in and reloads it. In practice this only matters on a literally shared device — each buyer using their own phone/browser is unaffected.
- Sync still requires the exact same Firestore rules update below (the `approved` field is still protected — only you can change it from the console).

## ✅ Approving a buyer after payment

There are now **two independent fields** on each buyer's Firestore document — approve each one separately depending on what they paid for:

| Field | Unlocks | Set to `true` when… |
|---|---|---|
| `approved` | The base tracker app (progress, scores, cloud sync) | They paid the base access fee |
| `aiApproved` | The 🤖 AI Study Assistant (Chat/Explain/Quiz/Advice) | They paid the AI add-on fee |

A buyer can have one, the other, both, or neither — whatever they've actually paid for.

1. Buyer opens your site → signs in with Google → sees a **"Almost there!"** pending screen (for the base app) with their email and a User ID, and instructions to pay you and message you. If they instead (or additionally) tap the 🤖 button without having paid for AI, they see a similar **AI Study Assistant** pending screen with the same kind of instructions, specific to the AI add-on.
2. Once they pay and message you (their message will say which one they're paying for), go to **Firebase Console → Firestore Database → `users` collection**.
3. Find the document matching their email (or the User ID they sent you).
4. Click into it → edit the relevant field(s):
   - Base access: change `approved` from `false` to `true`.
   - AI add-on: change `aiApproved` from `false` to `true`.
   → Save.
5. Their app unlocks **automatically within seconds** — they don't need to refresh or reinstall anything. This applies to both fields independently: approving `aiApproved` while they're mid-way through the AI paywall screen flips them straight into the AI chat.

To revoke access later, just flip the relevant field back to `false` the same way — you can revoke AI access without touching their base tracker access, or vice versa.

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
