# brewsterwickershampublications.com

Static site for Brewster Wickersham Publications. No build step, no
dependencies — edit the HTML and push.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site. All content and styling live here. |
| `logo.jpg` | The BWP crest. Used in the header and as the social share image. |
| `favicon.png`, `favicon.ico`, `apple-touch-icon.png` | Browser-tab and search-result icon, cropped from the crest in `logo.jpg`. |
| `404.html` | Shown for bad URLs. |
| `products/`, `collections/`, `pages/`, `blogs/`, `policies/` | Redirects from the old Shopify store's addresses. See below. |
| `CNAME` | Tells GitHub Pages to serve the custom domain. **Do not delete.** |
| `.nojekyll` | Stops GitHub from running Jekyll over the files. |
| `robots.txt` | Search-engine directives; points at the sitemap. |
| `sitemap.xml` | Single-page sitemap. Update `lastmod` on major changes. |
| `.gitignore` | OS and editor cruft. |

## Editing

Everything is in `index.html`. Search for `EDIT ME` to find the spots that
need attention:

1. **Amazon links** — live. The URLs appear in three places: the `AMAZON`
   object in the script at the bottom, each button's `href`, and the
   structured data (`application/ld+json`) in `<head>`. Change all three if a
   listing URL changes.

Resilient Path, RheumCompanion, and SootheQuest are live and linked. **Lumina: The Cloud Garden** is listed as
"Coming soon" with an email-us link; when it launches, change its badge to
`<span class="badge">Available now</span>`, add pricing, and add store buttons
like the other app cards.

Colors are the CSS variables at the top of the `<style>` block, drawn from the
logo: navy `#1a2b4a`, gold `#8a6a2b`, cream `#f8f4ea`. Changing those re-skins
the whole site, light and dark modes together.

### Still to confirm

- **Module counts.** The app has 25 modules (confirmed in the app source,
  Module 1 through Module 25), and this site now says 25. If the App Store or
  Google Play listing still says 24, update the listing.
- **Book subtitle.** This site and theresilientpathbook.com use *Managing Life
  with Chronic Pain*. The SootheQuest App Store listing calls it *Modern
  Strategies for Living with Chronic Pain*. One of them is stale.
- **Share image.** `logo.jpg` (512×279) is serving as the Open Graph image.
  A purpose-made 1200×630 version would look better when the site is shared
  on social platforms.

## Old store addresses

This domain used to be a Shopify store, and search engines still list some of
its pages. Each folder below holds a small page at the old address that sends
visitors (and Google, which treats an instant meta refresh as a permanent
redirect) to the right place on this site:

| Old address | Goes to |
|---|---|
| `/products/managing-life-with-chronic-pain-the-resilient-path` | theresilientpathbook.com |
| `/collections/all` | `/#books` |
| `/pages/contact` | `/#contact` |
| `/blogs/news` | `/` |
| `/policies/refund-policy`, `terms-of-service`, `privacy-policy`, `shipping-policy` | `/#contact` |

They are deliberately left out of `sitemap.xml`. To redirect another old
address, copy one of these files to the matching path (`/pages/about` →
`pages/about.html`) and change the three URLs inside it.

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
