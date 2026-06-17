# ECSB Project Tracking — Setup Guide

This guide takes your app from a file on your computer to a **live link your team can use** and an **Android app (APK)** you can share. No coding needed — just follow the steps in order.

You do this **once**. It takes about 20–30 minutes.

There are 3 parts:
- **Part A** — Create a free online database (Firebase)
- **Part B** — Put the app online and share the link
- **Part C** — Make the Android APK

---

## Before you start

Inside the ZIP you have 5 files. Keep them together in one folder:
`index.html`, `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`

You only ever edit **one** of them: `index.html`.

---

## Part A — Create the free database (Firebase)

The app needs one shared online database so all 40 team members see the same projects and deadlines.

1. Go to **https://console.firebase.google.com** and sign in with a Google account (use a company Google account if you have one).
2. Click **Add project**. Name it `ECSB Project Tracking`. You can turn **off** Google Analytics. Click **Create**.
3. On the left menu, open **Build → Firestore Database**. Click **Create database**.
   - Choose location **asia-southeast1 (Singapore)** — closest to Malaysia.
   - Start in **Production mode**. Click **Enable**.
4. Open the **Rules** tab inside Firestore, delete what's there, paste the text below, then click **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /app/data { allow read, write: if true; }
       match /{document=**} { allow read, write: if false; }
     }
   }
   ```

5. Now get your keys. Click the **gear icon (Project settings)** at top-left → scroll to **Your apps** → click the **web icon `</>`**.
   - App nickname: `ECSB Track`. **Do not** tick Firebase Hosting. Click **Register app**.
   - You'll see a block of text called `firebaseConfig` with lines like `apiKey: "..."`. **Keep this page open** — you need these values next.

6. Open `index.html` in a text editor (Notepad on Windows is fine). Near the top of the code, find this block:

   ```
   const FB_CONFIG = {
     apiKey: "PASTE_API_KEY",
     authDomain: "PASTE_PROJECT.firebaseapp.com",
     ...
   };
   ```

   Replace each `PASTE_...` value with the matching value from your Firebase page. Copy them exactly, keep the quotation marks. Save the file.

That's the database done. ✅

---

## Part B — Put the app online and share the link

The easiest free way, no account juggling:

1. Go to **https://app.netlify.com/drop**
2. Drag your whole folder (all 5 files) onto the page.
3. Wait a few seconds — Netlify gives you a live web link like `https://something.netlify.app`.
4. (Recommended) Create a free Netlify account when prompted so the link stays permanent, and you can rename it to something like `ecsb-track.netlify.app`.

**Share that link with your team.** On an Android phone they open the link, then tap the browser menu → **Add to Home screen**. It then opens full-screen like a real app, with your EC logo.

### First login
- Open the link and sign in with **`admin` / `admin123`**.
- Go to **Settings** and change the admin password immediately.
- Go to **Team** and add your members (up to 40). Give each person a username and password — that's what they use to log in.

---

## Part C — Make the Android APK (optional)

If you specifically want an installable `.apk` file to send to your team:

1. Make sure Part B is done and your link works.
2. Go to **https://www.pwabuilder.com**
3. Paste your Netlify link and click **Start**.
4. When the report finishes, click **Package for stores → Android**.
5. Choose the simple option, click **Generate**, and download the package. Inside you'll get an installable Android file.
6. Send that file to your team. To install it, a phone must allow "Install unknown apps" for the app they open it with (this is normal for apps not from the Play Store).

If you'd rather publish on the **Google Play Store**, that needs a one-time Google Play developer account (about USD $25, one payment) and uploading the package PWABuilder made. A developer can do this in under an hour.

---

## Important notes on cost and safety

- **Cost: RM 0.** Firebase's free plan covers 40 users and this amount of data with huge room to spare, and it does **not** pause your project.
- **Login security:** Passwords are scrambled (hashed) before saving, not stored as plain text. This is good for an internal team tool. Because there is no SMS/email verification, treat this as an internal tool and avoid storing confidential information in it.
- **Want stronger security later?** The next step up is "Firebase Authentication," which adds proper account protection. Ask and this can be built on top of what you have.
- **Backups:** Your data lives in Firebase. Don't delete the Firebase project.

---

## Quick troubleshooting

- **Team members don't see each other's projects** → the Firebase config in `index.html` wasn't pasted correctly, or the folder you uploaded had the old `index.html`. Re-check Part A step 6 and re-upload.
- **"Permission denied" errors** → the Firestore Rules in Part A step 4 weren't published. Redo that step.
- **Logo not showing** → make sure all 5 files were uploaded together, not just `index.html`.
