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

## 📜 License

This project is **proprietary** — not open source. See `LICENSE.md`. Copying, redistributing, reselling, or creating derivative versions without written permission is not allowed. Every file also carries a short copyright header so the notice travels with the code even if someone downloads individual files.

If you ever find someone hosting a copy of this app (or an obvious clone) elsewhere, having this LICENSE file in your repo — plus your GitHub commit history, which timestamps when you first published each version — is what you'd point to when filing a takedown request (e.g. GitHub's DMCA process at https://github.com/contact/dmca) or a takedown with whatever host they're using. This is general information, not legal advice — for anything beyond a straightforward takedown request, it's worth talking to a lawyer.

## 📄 Privacy Policy & Terms of Service

`privacy.html` and `terms.html` are standalone pages (linked from the sign-in screen) covering what data the app collects, how it's used, your payment/refund policy, acceptable use, and AI-assistant disclaimers. They're a solid starting draft, but:

- Update the contact name/details if they're not accurate.
- These are general-purpose drafts, not legal advice tailored to your specific situation — for a one-person paid app like this they should already cover the basics reasonably well, but if you want extra peace of mind (especially around refund policy or Philippine consumer protection law specifics), it's worth a quick review by a lawyer.
- Both pages update their own "Last updated" date manually — bump it if you make meaningful changes later.


## Subjects Covered
| Subject | Description |
|---------|-------------|
| NP I | Community Health Nursing |
| NP II | Maternal, Child & Adolescent Nursing |
| NP III | Surgery, Oxygenation, F&E, Immunology |
| NP IV | GI, Endocrine, Neuro, MSK & Senses |
| NP V | Psychiatric Nursing & Emergency Care |
