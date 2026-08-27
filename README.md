# Alfaytri Apps — launcher (`apps.alfaytri.com`)

A single static page that lists every Alfaytri app. Clicking a tile opens that app,
where the user signs in. There is **no build step** — it's plain HTML/CSS.

```
alfaytri-apps/
├── index.html     ← the launcher page (edit tiles here)
├── logo.png       ← Al Faytri logo (header + browser-tab icon)
├── vercel.json    ← static hosting config + security headers
└── README.md      ← this file
```

---

## Deploy to Vercel

Pick **one** of the two ways.

### Option A — Vercel CLI (fastest, no Git needed)

```bash
npm i -g vercel
cd D:\alfaytri-apps
vercel --prod
```

When prompted: create a **new project** (name it e.g. `alfaytri-apps`), framework preset
**Other**, accept the defaults. It deploys and prints a `*.vercel.app` URL.

### Option B — Git + Vercel import (nicer for ongoing edits)

1. Create a new **empty** GitHub repo (e.g. `alfaytri-apps`).
2. In this folder:
   ```bash
   git init
   git add .
   git commit -m "Alfaytri Apps launcher"
   git branch -M main
   git remote add origin https://github.com/<you>/alfaytri-apps.git
   git push -u origin main
   ```
3. In Vercel → **Add New… → Project → Import** the repo. Framework preset: **Other**.
   Every future `git push` redeploys automatically.

---

## Point `apps.alfaytri.com` at it

1. In the new Vercel project → **Settings → Domains** → add `apps.alfaytri.com`.
2. In **GoDaddy → DNS → Add New Record**:

   | Type  | Name   | Data                    | TTL         |
   |-------|--------|-------------------------|-------------|
   | CNAME | `apps` | `cname.vercel-dns.com`  | 600 seconds |

   (Use whatever target Vercel shows — it's usually `cname.vercel-dns.com`.)
3. Wait a few minutes → Vercel shows **Valid Configuration** and issues SSL.
   `https://apps.alfaytri.com` is live.

---

## Editing the page

Open `index.html`. Every app is one card in the `.grid`.

- **Add a live app:** copy an existing `<a class="app" href="https://…">` block, change the
  icon, name, description, and `href`.
- **Turn a "Coming soon" app live:** change its `<div class="app soon">` to
  `<a class="app" href="https://service.alfaytri.com">`, remove the `soon` class,
  and swap the `Not yet available` span for the `Open` span + a `Live` pill.
  (There's a comment in the file showing exactly this.)

Save, then redeploy (`vercel --prod`, or `git push` if you used Option B).

---

## Notes

- The page is marked `noindex` so it won't appear in Google. Remove the
  `<meta name="robots" …>` line in `index.html` if you ever want it public.
- It's a public directory of links (no login on the launcher itself). The apps it
  links to stay protected by their own sign-in. If you later want the launcher
  behind a login too, that's a future upgrade (true single sign-on).
