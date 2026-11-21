# Levifiny Inc. website

The company website for **Levifiny Inc.**, for **levifiny.com**. It is plain HTML and CSS: no build step, no scripts, no cookies, no analytics, no outside fonts.

It does three jobs:

1. It is the public website on the company's own domain that Apple asks an organisation to have when it enrols in the Apple Developer Program.
2. It gives the Batchbell App Store listing its **Privacy Policy URL** and **Support URL**.
3. It introduces the company and its apps, in English and French.

## Pages

| Page | English | French |
| --- | --- | --- |
| Home | `/` | `/fr/` |
| Apps | `/apps/` | `/fr/applications/` |
| Batchbell (with support and FAQ) | `/batchbell/` | `/fr/batchbell/` |
| Batchbell privacy policy | `/batchbell/privacy/` | `/fr/batchbell/confidentialite/` |
| Contact | `/contact/` | `/fr/contact/` |
| Not found | `/404.html` (both languages) | |

Each page is an `index.html` in its folder, so the addresses end in `/`. Every page links to its other-language twin (the EN/FR button in the header).

For App Store Connect:

- **Privacy Policy URL:** `https://levifiny.com/batchbell/privacy/`
- **Support URL:** `https://levifiny.com/batchbell/#support`
- **Marketing URL (optional):** `https://levifiny.com/batchbell/`
- **App Privacy (the "nutrition label"):** answer *No, we do not collect data from this app*, which shows as **Data Not Collected**. It matches the policy: Batchbell has no account, server, analytics or ads, and speech recognition is Apple's own service.

For the Apple Developer Program enrolment, the website to give is `https://levifiny.com`.

## Before publishing

- [ ] **Business address.** `contact/index.html` and `fr/contact/index.html` each have a dashed orange box marked `TODO ADDRESS`. Replace the whole `<div class="placeholder">…</div>` with the address, for example:

  ```html
  <address>123 Example Street<br>Ottawa, Ontario  K1A 0A1<br>Canada</address>
  ```

  Search the folder for `TODO ADDRESS` to find both. Use the address you want public (it does not have to be the registered office), and keep it the same as on your D-U-N-S record if Apple compares them.
- [ ] **Read every page once in both languages.** The French is Canadian French in the same wording as the Batchbell app.
- [ ] When Batchbell is live, change "Coming soon to the App Store" / "Bientôt sur l'App Store" (Home, Apps and Batchbell pages, both languages) to a link to its App Store page.

What the site deliberately leaves out: shareholders, share details, the corporation number, directors and anything else from the incorporation documents beyond the name "Levifiny Inc." and the province.

## Editing

Edit the HTML files directly. The header and footer are repeated in every page, so a change to them (a new menu item, say) goes in all ten pages; search and replace across the folder works well. All styling is in `assets/site.css`:

- Company pages use the Levifiny **Sun & Clouds** colorway (Second Proposal, Appendix C): Clear Sky `#52AEDD`, Sky Surface `#CCE8F8`, Sun Core White `#FDF8E0`, Horizon Amber `#F09030`, Sun Halo Gold `#F8D060`, Deep Sky `#2E5270`, Mountain Mist `#8AAEC8`.
- Batchbell pages have `<body class="bb">`, which switches to Batchbell's palette: Warm Cream `#F8E8D2`, Bell Gold `#F9B536`, Terracotta `#E97835`, Sage Olive `#92936B`, Coffee Brown `#59351F`, Espresso `#2E1D13`.

Images in `assets/img/` are copies: the Levifiny wordmark and icon from `002-Levifiny`, and the Batchbell bells and app icon from the Batchbell repo (`assets/images/`). If either logo changes, copy the new file over the old one.

The privacy policy describes what the Batchbell app does today. If the app ever starts sending anything off the phone (crash reports, analytics, an account, a server), update `batchbell/privacy/` and `fr/batchbell/confidentialite/`, their "Last updated" dates, and the App Store privacy answers **before** that version ships.

## Preview on your computer

Links between pages end in `/`, so open the site through a small local server rather than double-clicking a file. From this folder:

```powershell
python -m http.server 8000
```

Then open <http://localhost:8000>. Press Ctrl+C to stop it.

## Publishing (free)

Pick one host. Both are free for a site like this, give HTTPS automatically and work with a domain you already own. Nothing here is published until you do these steps.

### Option A: Cloudflare Pages (simplest if the domain's DNS is, or can move, at Cloudflare)

1. Create a free Cloudflare account.
2. **Workers & Pages → Create → Pages → Upload assets** (or *Connect to Git* if you push this folder to GitHub). Name the project `levifiny` and upload this whole folder. Leave the build command empty and the output directory as the folder root.
3. Open the project → **Custom domains → Set up a custom domain** → `levifiny.com`, then again for `www.levifiny.com`.
   - If levifiny.com's DNS is already on Cloudflare, it adds the records for you.
   - If not, Cloudflare shows the records to add at your registrar (a `CNAME` for `www` pointing at `levifiny.pages.dev`; for the bare domain it asks you to move the nameservers to Cloudflare, which is free and the easiest way to point the bare domain).
4. To send `www` to the bare domain: **Rules → Redirect Rules** (or *Bulk Redirects*) → `www.levifiny.com/*` → `https://levifiny.com/${1}`, status 301.
5. The `CNAME` file in this folder is for GitHub Pages and is harmless on Cloudflare.

### Option B: GitHub Pages

1. Create a **public** repository on GitHub (free accounts need public repos for Pages), for example `levifiny-website`, and push this folder to it:

   ```powershell
   git remote add origin https://github.com/<your-account>/levifiny-website.git
   ```

   ```powershell
   git push -u origin main
   ```

2. In the repository: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch `main`, folder `/ (root)`, Save.
3. Still in **Settings → Pages**, set **Custom domain** to `levifiny.com` (the `CNAME` file in this folder already says so). Recommended first: in your GitHub account's **Settings → Pages**, verify `levifiny.com`, so nobody else can claim it.
4. At your domain registrar, set these DNS records for levifiny.com (delete any old `A`, `AAAA` or parking records for `@` and `www` first):

   | Type | Name | Value |
   | --- | --- | --- |
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |
   | AAAA | @ | 2606:50c0:8000::153 |
   | AAAA | @ | 2606:50c0:8001::153 |
   | AAAA | @ | 2606:50c0:8002::153 |
   | AAAA | @ | 2606:50c0:8003::153 |
   | CNAME | www | `<your-account>.github.io` |

   GitHub then redirects `www.levifiny.com` to `levifiny.com` by itself. Check these addresses against GitHub's current guide ("Managing a custom domain for your GitHub Pages site") before entering them.
5. When the DNS has spread (minutes to a day), tick **Enforce HTTPS** in Settings → Pages.

### Email on the domain

Pointing the website at a host does not touch email. If levifiny.com has `MX` records (or you add some later for an address like `hello@levifiny.com`), leave them as they are. The site uses levifiny@gmail.com today.

### After it is live

- Open `https://levifiny.com`, `https://www.levifiny.com` and `https://levifiny.com/batchbell/privacy/` on a phone and a computer and check the padlock.
- Put the privacy and support URLs into App Store Connect.
- Use `https://levifiny.com` as the company website in the Apple Developer Program enrolment.

© 2026 Levifiny Inc.
