# Lilac Kat Creatives

Portfolio and commission site for Kat's art: `lilackatcreatives.com`.

## Structure
- `index.html` — the full site (HTML/CSS/JS in one file)
- `robots.txt`, `sitemap.xml` — basic SEO plumbing
- `CNAME` — custom domain for GitHub Pages
- `.nojekyll` — tells GitHub Pages to serve files as-is, no Jekyll processing

## Local preview
No build step. Open `index.html` directly in a browser, or serve it:
```
python3 -m http.server 8000
```

## Deploying (GitHub Pages)
This repo is set up to deploy straight from GitHub Pages — no separate host needed:
1. On GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch**.
2. Branch: `main`, folder: `/ (root)`. Save.
3. Under **Custom domain**, GitHub will pick up the `CNAME` file automatically once it detects it, or enter `lilackatcreatives.com` there yourself and check **Enforce HTTPS**.
4. Point the domain's DNS at GitHub Pages — see [Connecting the Squarespace domain](#connecting-the-squarespace-domain) below.

If you'd rather host elsewhere, this is also a drop-in fit for Netlify or Vercel (root directory, no build command) — same DNS pattern applies, pointed at that host's records instead.

## Connecting the Squarespace domain

Squarespace is only the domain registrar here — DNS for the domain is managed in the
Squarespace **Domains** panel, not in Squarespace's website builder.

1. Log into Squarespace → **Domains** → select `lilackatcreatives.com` → **DNS Settings**.
2. Remove any existing A/ALIAS/CNAME records that point at Squarespace's own website hosting (if the domain was ever connected to a Squarespace-built site).
3. Add these records for GitHub Pages:

   **Apex domain** (`lilackatcreatives.com`) — four A records, all with host `@`:
   | Type | Host | Data |
   |------|------|------|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |

   **www subdomain** (`www.lilackatcreatives.com`) — one CNAME:
   | Type | Host | Data |
   |------|------|------|
   | CNAME | www | `schneidvegas.github.io.` |

4. Save. DNS propagation is usually under an hour, but can take up to 24-48 hours.
5. Back in GitHub → **Settings → Pages**, confirm the custom domain shows a green check, then enable **Enforce HTTPS** (GitHub issues the certificate automatically once DNS resolves — this can take a few minutes to an hour after step 4).
6. Verify: visit `http://lilackatcreatives.com` and `https://lilackatcreatives.com` and confirm both load and redirect to https.

If deploying to Netlify or Vercel instead, use that host's own apex/CNAME target in step 3 (both show you the exact records to add once you add the domain in their dashboard) — the Squarespace-side steps 1-2 and 4-6 are the same.

## Grooming backlog
- [x] Swap the "pets" placeholder `.art-canvas` tiles for real photos — still needed for portraits/fantasy/sketches
- [ ] Replace the About section bio text with Kat's own
- [ ] **Commission form needs a real Formspree ID.** The form in `#commission` posts to `https://formspree.io/f/YOUR_FORM_ID`, a placeholder — it will not send anywhere until you:
  1. Sign up free at [formspree.io](https://formspree.io), create a form, and confirm the notification email.
  2. Copy the real endpoint it gives you (`https://formspree.io/f/xxxxxxxx`) and replace `YOUR_FORM_ID` in `index.html`'s `<form action="...">`.
  3. Check Formspree's current plan limits for file uploads (attachment size/count caps vary by plan and change over time) — the site enforces its own client-side limit (5 files, 10MB each, set via `MAX_COMMISSION_FILES`/`MAX_COMMISSION_FILE_MB` in the `<script>` at the bottom of `index.html`), but the form will still fail silently if Formspree's own limit is lower. Adjust the JS constants to match whatever Formspree actually allows.
  4. The `mailto:hello@lilackatcreatives.com` fallback link under the form works today regardless, so the page isn't broken in the meantime.
- [ ] The Instagram link in the About section (`<a href="#" class="instagram-link">`) is a placeholder — point it at Kat's real profile URL once she has one to link.
- [ ] Add real Open Graph / social preview image (currently text-only meta tags)
- [ ] Add a favicon/social image once brand assets exist (currently an inline placeholder "K" mark)
