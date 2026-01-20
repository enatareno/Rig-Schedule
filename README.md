# Rig Schedule App (Internal)

This is a lightweight internal multi-rig schedule web app backed by Firebase Firestore.

## Features
- Multi-rig schedule table (sortable by start date via Firestore order)
- Filters: Rig (multi-select), Status, Contractor, Engineer, Date range, Search
- Click a row to edit job fields in a side drawer
- Conflict flagging (overlapping date ranges on the same rig)
- Excel importer built-in (upload .xlsx, preview, import to Firestore)
- Lat/Long is intentionally blank (null) in every job document

---

## 1) Install
```bash
npm install
npm run dev
```

---

## 2) Firebase setup
1. Go to Firebase Console -> create/select project
2. Create a Firestore Database (production or test mode for internal)
3. Add a Web App in Firebase settings to get config values

Create a file named `.env.local` in the project root:

```bash
VITE_FIREBASE_API_KEY=YOUR_KEY
VITE_FIREBASE_AUTH_DOMAIN=YOUR_DOMAIN
VITE_FIREBASE_PROJECT_ID=YOUR_PROJECT_ID
VITE_FIREBASE_STORAGE_BUCKET=YOUR_BUCKET
VITE_FIREBASE_MESSAGING_SENDER_ID=YOUR_SENDER_ID
VITE_FIREBASE_APP_ID=YOUR_APP_ID
```

Then restart the dev server.

---

## 3) Import your Excel
Open the **Import Excel** tab, upload your file, preview the rows, then import.

---

## 4) Recommended Firestore Rules (internal)
If you will use Firebase Auth later, lock this down.
For quick internal use (you only), you can start simple:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /jobs/{doc} {
      allow read, write: if true; // tighten later
    }
  }
}
```

---

## Notes
- If your Excel headers change, the importer tries to map them using synonyms.
- If Status is blank, it defaults to Planned.
- If Duration looks like "68d", it will import as 68.
