# 🐾 Cat Paw Visit Book — GitHub Pages Edition

A zero-backend visitor stamp book. Every stamp is saved as a commit to
`stamps.json` in this repo via the GitHub Contents API.

**No server. No database. Completely free.**

---

## How it works

```
Visitor types name → clicks stamp
  → site fetches current stamps.json from GitHub API (with SHA)
  → appends new stamp entry
  → PUTs updated file back → GitHub creates a new commit
  → stamps.json is updated in the repo
  → next visitor sees the new stamp on load
```

---

## Setup (5 minutes)

### 1 — Fork / create this repo on GitHub

Keep it **public** so GitHub Pages works on the free plan.

### 2 — Enable GitHub Pages

Repo → **Settings → Pages → Source → GitHub Actions**

### 3 — Create a fine-grained Personal Access Token

1. GitHub → **Settings** (your profile, top-right)
2. **Developer Settings → Personal access tokens → Fine-grained tokens → Generate new token**
3. Set:
   - **Token name:** `catpaw-stamp-writer`
   - **Expiration:** choose how long you want the site to accept stamps
   - **Repository access:** Only select repositories → pick this repo
   - **Permissions → Contents → Read and write**
4. Click **Generate token** — copy it now (you won't see it again)

### 4 — Edit `index.html`

Open `index.html` and fill in the three lines at the top of the first `<script>` block:

```js
const GITHUB_OWNER = "your-github-username";
const GITHUB_REPO  = "catpaw-site";           // this repo's name
const GITHUB_TOKEN = "github_pat_xxxxxxxxxxxx"; // token from step 3
```

**Optional:** swap in your cat's paw image:
```js
const CUSTOM_PAW_IMAGE = "/paw.png";  // drop paw.png into the repo root
```

### 5 — Push & deploy

```bash
git add .
git commit -m "init: cat paw visit book"
git push origin main
```

GitHub Actions runs automatically and your site is live at:
`https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/`

---

## File structure

```
catpaw-site/
├── index.html          ← entire site (HTML + CSS + JS)
├── stamps.json         ← all visitor stamps (auto-updated by the site)
├── paw.png             ← (optional) your cat's paw image
└── .github/
    └── workflows/
        └── deploy.yml  ← auto-deploys on every push
```

---

## Security notes

| Concern | Reality |
|---|---|
| Token visible in source | Yes — it's in the HTML. Use a fine-grained token scoped **only to this repo** with **only Contents: write** permission. Worst case: someone spams stamps. |
| Rate limiting | GitHub API: 5,000 requests/hour per token — more than enough |
| Concurrent writes | The site re-fetches the latest SHA before each write, so conflicts are rare. If two people stamp simultaneously one gets a 409 and is prompted to retry. |

---

## Customise your paw

Edit line 8 of the `<script>` block in `index.html`:

```js
const CUSTOM_PAW_IMAGE = "/paw.png";
```

Drop `paw.png` in the repo root. Use a **transparent PNG** for the best ink-stamp look.

---

## Reset stamps

To clear all stamps:

```bash
echo "[]" > stamps.json
git add stamps.json
git commit -m "reset stamps"
git push
```
