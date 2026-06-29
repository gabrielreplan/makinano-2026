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

## 🌐 FREE Hosting on GitHub Pages (Step-by-Step)

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
