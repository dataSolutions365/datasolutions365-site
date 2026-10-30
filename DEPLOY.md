# Deploying datasolutions365.com on GoDaddy

The whole site is one file: `index.html` (CSS and JavaScript are inside it, charts are drawn in the browser, no images to upload).

## Before you start: back up what's there
If something is already live at datasolutions365.com, download a copy of the current `index.html` (or `index.php`) first so you can roll back.

## Option A: GoDaddy Web Hosting (cPanel) — recommended
1. Sign in at godaddy.com → **My Products** → next to **Web Hosting**, click **Manage**.
2. Click **cPanel Admin** → open **File Manager**.
3. Open the **public_html** folder (this is the root for datasolutions365.com).
   - Your games likely live in their own folders, e.g. `public_html/dressup` and `public_html/snake`, or wherever each subdomain's "Document Root" points (check under cPanel → **Domains**). Don't touch those.
4. If there's an existing `index.html`, `index.php` or `default.html`, rename it (e.g. `index-old.html`). Hosting serves `index.php` before `index.html` in some setups, so an old `index.php` must be renamed.
5. Click **Upload** and upload the new `index.html`.
6. Visit https://datasolutions365.com and hard-refresh (Ctrl+F5).
7. Click **🎮 Games** in the top menu and test both game links.

FTP alternative: in cPanel → **FTP Accounts**, create/use an account, then upload `index.html` to `/public_html` with FileZilla.

## Option B: GoDaddy Website Builder (Websites + Marketing)
Website Builder doesn't let you upload a full HTML page. Choices:
- Add an **HTML** section (Add Section → search "HTML") and paste the parts you want; the games links can also be added as a simple **Button** section pointing at the two game URLs, or as nav links (Site Navigation → Add Link).
- Or move the domain to GoDaddy Web Hosting / Azure (below) and use this file as-is.

## Option C: Azure Static Web Apps (good path toward Blazor)
1. Put `index.html` in a GitHub repo.
2. Azure Portal → Create **Static Web App** (Free plan) → connect the repo, app location `/`.
3. In the Static Web App → **Custom domains** → add `datasolutions365.com` and `www.datasolutions365.com`.
4. In GoDaddy → **DNS** for the domain, add the TXT/CNAME (or ALIAS) records Azure shows you.
   - Leave the existing `dressup` and `snake` DNS records alone so the games keep working.
A Blazor WebAssembly version can be deployed the same way later.

## Making it yours (quick edits in index.html)
- **Email:** search for `info@datasolutions365.com` and replace with your real address.
- **Products:** search for `Sales Dashboard Starter`, `SQL Health Check`, `Reporting Support Plan` to change names, prices and bullet points. Change each `href="#contact"` to a checkout or product link (e.g. Stripe, Gumroad, PayPal) when ready.
- **Games:** search for `dressup.datasolutions365.com` / `snake.datasolutions365.com`. To add a third game, copy one `<a class="game ...">…</a>` block.
- **Chart numbers:** at the bottom in the `<script>`, the arrays like `const rev = [...]` hold the sample data.
