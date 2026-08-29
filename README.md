# brewsterwickershampublications.com

Static site for Brewster Wickersham Publications. No build step, no
dependencies — edit the HTML and push.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site. All content and styling live here. |
| `logo.jpg` | The BWP crest. Used in the header and as the social share image. |
| `404.html` | Shown for bad URLs. |
| `CNAME` | Tells GitHub Pages to serve the custom domain. **Do not delete.** |
| `.nojekyll` | Stops GitHub from running Jekyll over the files. |
| `robots.txt` | Search-engine directives; points at the sitemap. |
| `sitemap.xml` | Single-page sitemap. Update `lastmod` on major changes. |
| `.gitignore` | OS and editor cruft. |

## Editing

Everything is in `index.html`. Search for `EDIT ME` to find the spots that
need attention:

1. **Amazon links** (2×) — the book and workbook. Replace `href="#"`, remove
   `class="disabled"`, change the label to "Buy on Amazon".

That is the only placeholder left. All three apps are live and linked.

Colors are the CSS variables at the top of the `<style>` block, drawn from the
logo: navy `#1a2b4a`, gold `#8a6a2b`, cream `#f8f4ea`. Changing those re-skins
the whole site, light and dark modes together.

### Still to confirm

- **Module counts.** The workbook has 25 modules; the Resilient Path app's
  store listing says 24. These may genuinely differ — worth a look.
- **Book subtitle.** This site and theresilientpathbook.com use *Managing Life
  with Chronic Pain*. The SootheQuest App Store listing calls it *Modern
  Strategies for Living with Chronic Pain*. One of them is stale.
- **Share image.** `logo.jpg` (512×279) is serving as the Open Graph image.
  A purpose-made 1200×630 version would look better when the site is shared
  on social platforms.

## Deploying to GitHub Pages

### 1. Create the repo

Create a new **public** repository on GitHub. Any name works — the custom
domain is what visitors see. `brewsterwickershampublications.com` is a
reasonable name.

Then, from inside this folder:

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/pennwickersham/REPO-NAME.git
git push -u origin main
```

### 2. Turn on Pages

Repo → **Settings** → **Pages**

- Source: **Deploy from a branch**
- Branch: **main**, folder: **/ (root)**
- Save

Because `CNAME` is in the repo, GitHub picks up the custom domain
automatically. Wait for the "Your site is live" message.

### 3. DNS at Porkbun

Add these records. **Do not touch the existing MX or TXT records** — those are
Google Workspace email and will break mail if removed.

Four **A** records on the root (Host left blank):

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Four **AAAA** records on the root, also blank Host (optional but recommended):

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

One **CNAME**, Host `www`:

```
pennwickersham.github.io
```

### 4. Enable HTTPS

Back in Settings → Pages, wait for the certificate to issue (usually minutes,
occasionally up to an hour), then tick **Enforce HTTPS**.

### 5. Verify

```bash
nslookup brewsterwickershampublications.com 8.8.8.8
```

```bash
nslookup -type=MX brewsterwickershampublications.com 8.8.8.8
```

The second command must still list all five `aspmx.l.google.com` hosts. If it
comes back empty, an MX record was deleted by mistake — restore it before
anything else.

## Making changes later

Edit `index.html`, then:

```bash
git add -A
git commit -m "Describe the change"
git push
```

GitHub Pages redeploys in about a minute.
