# Coding Concepts — Learning Studio

A nine-module, self-contained interactive study book covering the Python data-science stack. Every chapter follows the same pattern: a **W/H concept table** (What / Why / How / When / Where), a **mnemonic**, a **copyable code example**, and a **five-step learning cycle** ending in a checklist and reflection notes.

## Architecture (v2 — data-driven)

This version separates **content** from **presentation** so editing a chapter is a small, clean git diff instead of touching an 800-line HTML file:

```
index.html          — landing page shell (renders itself from data/chapters.json)
chapter.html         — single shared chapter template (?ch=1 … ?ch=9)
assets/
  styles.css         — all visual styling, shared by every page
  app.js             — renders chapters/index from JSON, syntax-highlights code,
                       handles the copy button and localStorage persistence
data/
  chapters.json      — one row per chapter: id, module number, title, tagline, nav label
  chapter1.json …     — full content for each chapter: concept table rows, mnemonic,
  chapter9.json        code, sandbox note, and the five learning-cycle steps
wrangler.toml        — Cloudflare Pages project config (for CLI deploy)
package.json         — `npm run start` / `npm run deploy` convenience scripts
```

**To edit a chapter:** open `data/chapterN.json` and change the text. No HTML or JS to touch. **To add a chapter:** copy an existing `data/chapterN.json`, give it a new `id`, and add a matching entry to `data/chapters.json` — it'll appear in the nav and the index grid automatically.

## Running it locally

Because pages load their content with `fetch()`, opening `index.html` directly from disk (`file://`) will fail in most browsers (CORS blocks local JSON reads). Run a tiny local server instead:

```bash
python3 -m http.server 8000
# or: npm start
```

Then open `http://localhost:8000/`.

## Deploying

It's a static site — no build step — so any static host works. Three options:

### GitHub Pages
1. Push this repo to GitHub (or use the web upload flow — see below).
2. Repo Settings → Pages → Source: deploy from branch → `main` / root.
3. Live at `https://<you>.github.io/<repo>/`.

**No git/command line?** On GitHub, create a new repo, then use "uploading an existing file" on the empty repo page and drag in every file/folder here (`index.html`, `chapter.html`, `assets/`, `data/`, etc.) in one batch, then commit.

### Cloudflare Pages — Dashboard (connected to Git)
1. Push this repo to GitHub/GitLab.
2. Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git → pick the repo.
3. Build settings: **Framework preset: None**, **Build command: (leave empty)**, **Build output directory: /**.
4. Deploy. Live at `https://<project>.pages.dev`.

### Cloudflare Pages — Direct upload (no Git needed at all)
1. Cloudflare dashboard → Workers & Pages → Create → Pages → **Upload assets**.
2. Drag this whole folder in (or the unzipped contents) and deploy.
3. Live at `https://<project>.pages.dev` immediately — re-upload the folder any time you edit a JSON file to push an update.

### Cloudflare Pages — CLI
```bash
npm install
npx wrangler login
npm run deploy
```

## How progress is saved

Each chapter's checklist and reflection notes are saved with `localStorage`, scoped per chapter (`coding-concepts-ch1` … `coding-concepts-ch9`). Nothing is sent to a server — progress lives only in the browser that opened it, and a "Reset chapter progress" button on each page clears that chapter's saved state.

## Author

Kamol Das · Microbiology, University of Chittagong

