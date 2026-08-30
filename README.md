# prtechnologiesus.com — static site

Five hand-written HTML pages, one stylesheet, one 6-line script. No build step, no
dependencies, no backend.

## Preview it locally

Double-clicking `index.html` works, but a local server is closer to how it will really
behave. From this folder:

```
python3 -m http.server 8000
```

Then open <http://localhost:8000>. Stop it with Ctrl-C.

Check these while you're looking:

- Every nav link and footer link goes somewhere.
- The email button opens your mail client with hr@prtechnologiesus.com filled in.
- Narrow the window below ~900px — columns should stack and the nav should collapse to
  the hamburger. Click it; the menu should open and close.
- Nothing should scroll sideways at any width.

## Files

```
index.html          Home
services.html       Services
ai-solutions.html   AI Solutions
about.html          About
contact.html        Contact
styles.css          The whole design system — colors, type, layout, responsive rules
site.js             Mobile nav toggle, nothing else
favicon.svg         The PR mark
robots.txt          Allows everything, points at the sitemap
sitemap.xml         The five pages
```

The header and footer are duplicated in each page. That is the cost of having no build
step — if you change the nav, change it in all five files. If that starts to hurt, that
is the moment to move to Astro or Eleventy, not before.

## Before you go live

Search the files for `[` — every bracketed value is a placeholder:

- `[CITY, STATE]` — footer, contact page, and the JSON-LD block in `index.html`
- `[YEAR]` — copyright line in every footer, founding year on About
- `[CLIENT LOGO]` — home page logo band (or delete the section until you have logos)
- `[NAME]` / `[TITLE]` / `[PHOTO]` — leadership cards on About
- `[PARTNER BADGE]` / `[CERTIFICATION]` — credentials on About
- `[PILOT FEE — CONFIRM BEFORE PUBLISHING]` — AI Solutions
- `[TERMS TO CONFIRM]` — staffing guarantee on Services

There is no phone number anywhere yet. When you have one, add a `tel:` link beside the
email in four places: the nav, each CTA band, the footer, and the contact page panel.

## Fonts

The pages load Space Grotesk, Instrument Sans, and IBM Plex Mono from Google Fonts. That
is fine to launch with. For a faster first paint later, download the woff2 files, drop
them in a `fonts/` folder, and replace the `<link>` with `@font-face` rules in
`styles.css`.

## Deploying to GitHub Pages

Two files here exist only for GitHub Pages:

- `CNAME` — contains `prtechnologiesus.com`. GitHub reads this to know the custom domain.
  Do not delete it; GitHub rewrites it if you change the domain in repo settings.
- `.nojekyll` — stops GitHub from running these files through Jekyll. Nothing here needs
  Jekyll, and it makes builds faster and more predictable.

### 1. Put the files in a public repo

The free plan serves Pages from public repositories only. That is fine — everything here
is public-facing anyway. Nothing secret should ever go in this folder.

Either drag the files into github.com → New repository → "uploading an existing file",
or from this folder:

```
git init -b main
git add .
git commit -m "Initial site"
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
git push -u origin main
```

### 2. Turn Pages on

Repo → Settings → Pages → Source: "Deploy from a branch" → Branch: `main`, folder: `/`.
Wait a minute, then the site appears at `YOUR-USERNAME.github.io/YOUR-REPO/`. Check it
there before touching DNS.

### 3. Add the custom domain

Same Settings → Pages screen, Custom domain: `prtechnologiesus.com` → Save.

### 4. Add the DNS records at Squarespace

Log in at squarespace.com with "Continue with Google" (the domain came through Google
Workspace), then Domains → prtechnologiesus.com → DNS.

Four A records on the apex, host `@`:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

One CNAME, host `www`, value `YOUR-USERNAME.github.io`

Delete the existing Squarespace parking records on `@` and `www`. **Leave every MX and
TXT record alone** — those carry Google Workspace mail and its SPF/DKIM/DMARC.

### 5. Enforce HTTPS

Back on Settings → Pages, tick "Enforce HTTPS" once it becomes available. It can take up
to 24 hours for the certificate to issue. An SSL warning before then is expected — wait
rather than changing anything.

## Updating the site later

Edit the files here, check locally, then commit and push. GitHub redeploys in under a
minute. If you uploaded through the web UI instead, upload the changed files again the
same way.
