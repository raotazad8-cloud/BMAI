# BookMate AI — NCTB Edition

Offline-first educational website for Bangladesh students (Class 1–12).  
Ultra Glass UI · Auto mobile/desktop layout · NCTB catalog · PDF reader · Local AI · IndexedDB uploads · Service Worker.

## Tech Stack

- HTML5, CSS3, JavaScript (ES Modules)
- Vite · PDF.js · IndexedDB · Service Worker
- Optional Transformers.js (browser local AI)
- Static deploy on **Vercel** · Books & catalog on **GitHub**

## Project Structure

```
BookMate-AI/
├── public/
│   ├── books/class-1/ … class-12/
│   ├── data/curriculum.json
│   └── sw.js
├── src/
│   ├── main.js
│   ├── style.css
│   ├── book-reader.js
│   ├── ai-assistant.js
│   └── curriculum.js
├── index.html
├── package.json
├── vite.config.js
├── vercel.json
└── README.md
```

## 1. Install Node.js

https://nodejs.org/ (LTS)

```bash
node -v
npm -v
```

## 2. Install & Run

```bash
cd BookMate-AI
npm install
npm run dev
```

Open the URL shown (usually http://localhost:5173).

## 3. Mobile vs Computer UI (automatic)

The app **detects the device** on load and on resize/orientation change:

| Device | Layout |
|--------|--------|
| **Phone / narrow screen** | Top bar + bottom navigation + slide-out menu |
| **Computer / wide screen** | Fixed left sidebar |

No manual switch needed. Classes `is-mobile` / `is-desktop` are set on `<body>`.

## 4. How to Add NCTB Books (GitHub)

Official books **must** live in the GitHub repository. The browser cannot push PDFs to GitHub.

### Option A — Manual (best for many books)

1. Put each PDF in the matching folder:

   ```
   public/books/class-1/bangla.pdf
   public/books/class-9/physics.pdf
   public/books/class-10/biology.pdf
   ```

2. Edit `public/data/curriculum.json` and add a subject under the right class:

   ```json
   {
     "id": "class-9-physics",
     "name": "Physics",
     "nameBn": "পদার্থবিজ্ঞান",
     "language": "bn",
     "year": 2026,
     "file": "/books/class-9/physics.pdf",
     "status": "official"
   }
   ```

3. Bump `"version"` at the top of the file (e.g. `1.0.0` → `1.0.1`).
4. Commit and push. Vercel redeploys → book appears in **NCTB Library**.

### Option B — Admin panel (prepare JSON, then publish)

1. Open the site → **Admin**.
2. Fill Class, Subject (EN / BN), Language, Year, PDF path  
   (example: `/books/class-9/physics.pdf`).
3. Click **Add Entry** (updates catalog **only in browser memory**).
4. Click **Download curriculum.json**.
5. In your repo, replace `public/data/curriculum.json` with the downloaded file.
6. Also upload the real PDF under `public/books/class-N/…`.
7. Commit & push → site updates.

**Important**

- Admin form does **not** write to GitHub.
- No API tokens in the frontend.
- Always: download JSON → commit → push.

### User uploads (not for NCTB publishing)

Use **Upload Book** in the app. Files stay in **IndexedDB on that device only**. Other users never see them; they are not published to GitHub.

## 5. curriculum.json format

```json
{
  "version": "1.0.1",
  "currentYear": 2026,
  "lastChecked": "2026-11-25",
  "classes": [
    {
      "class": 9,
      "subjects": [
        {
          "id": "class-9-physics",
          "name": "Physics",
          "nameBn": "পদার্থবিজ্ঞান",
          "language": "bn",
          "year": 2026,
          "file": "/books/class-9/physics.pdf",
          "status": "official"
        }
      ]
    }
  ]
}
```

- Sample entries use `"status": "example"`. Missing PDFs show a friendly empty state — no fake books.
- Keep older academic-year records if needed.

## 6. Deploy to Vercel

1. Push the project to GitHub.
2. https://vercel.com → **New Project** → import the repo.
3. Framework: **Vite** (or auto-detect).
4. Build: `npm run build` · Output: `dist`.
5. Deploy.

```bash
npm install
npm run build
```

`vercel.json` already configures SPA routing and cache headers.

## 7. Update the Website

1. Change source or `curriculum.json`.
2. Commit & push (Vercel rebuilds).
3. Or locally: `npm run build` then `npm run preview`.

## 8. Offline Reading

- Service Worker caches the app shell and catalog.
- User-uploaded books live in IndexedDB on the device.
- Previously opened books may remain available offline.
- New GitHub books and version checks need internet.
- Offline banner appears when the network is down.

## Features

| Feature | Status |
|---------|--------|
| Ultra Glass UI (light/dark) | ✅ |
| Auto mobile / desktop layout | ✅ |
| Class 1–12 catalog from JSON | ✅ |
| Friendly empty state for missing PDFs | ✅ |
| Custom PDF/TXT → IndexedDB | ✅ |
| PDF reader (zoom, pages, search, bookmarks) | ✅ |
| AI Assistant + honest search fallback | ✅ |
| Curriculum update checker | ✅ |
| Admin catalog + JSON download | ✅ |
| Service Worker offline | ✅ |
| বাংলা / English | ✅ |

## Security

- Upload validation (type + size ≤ 50 MB).
- User text escaped before rendering.
- No API keys or GitHub tokens in frontend.
- Private uploads never sent to external AI without permission (local model only).

---

**BookMate AI — NCTB Edition** · For Bangladeshi students · Offline-first · Privacy-respecting
