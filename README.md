# xavierbarbouche.com — build notes

Saved to the folder you connected: `~/Downloads/4614/Claude/`.

## What's here

- **`index.html`** — a complete, single-file personal site. No build step, no Ruby, no Jekyll. Open it in a browser to preview. Deploys to GitHub Pages, Cloudflare Pages, or Netlify as-is.

## Design choices, briefly

Clean, minimal, portfolio-forward. Ink/slate body, a single burnt-orange accent rule and section labels, Source Serif Pro for the name, Inter for body. Narrow content column (640px max), generous whitespace, one accent color. The Japanese subtitle under Xavier's name signals the Japan angle without making the whole site about it.

## What's placeholder — Xavier should edit before launch

Open `index.html` in any text editor (VS Code, TextEdit in plain-text mode, even Notes). The content lives in readable HTML — Xavier shouldn't need to touch CSS.

- **Email address** in the Contact section and the `mailto:` link. The file uses `xavier@xavierbarbouche.com` — if he wants that to be a real inbox, the domain needs email forwarding (Cloudflare and Namecheap both do this free). If he prefers to use his Gmail publicly, replace it.
- **GitHub username** in the footer link — currently `xavierbarbouche`. If his handle is different, edit it.
- **LinkedIn URL** in the footer — currently `xavier-barbouche`. Update to the real slug.
- **Role description.** The one-liner under his name — "Data scientist and data engineer. Between the U.S. and Japan." — is my take. If he wants it more specific (e.g., "Sports analytics and AI tooling. Based in [city].") swap it in.
- **About copy.** Two paragraphs. Drafted in his voice as I understand it from the cover letter and resume context. He should rewrite in his own words so it's actually his.

## Deploy path (recommended: GitHub Pages)

Three steps. Xavier does 1 and 2, you or I help with 3.

**1. Create the GitHub repo.**
   - Sign in to GitHub with the handle he wants. If it's `xavierbarbouche`, the repo that auto-serves at `xavierbarbouche.github.io` is called `xavierbarbouche.github.io`. Create that exact repo name, public, empty.
   - If his handle is different, we'll use a repo called `xavierbarbouche.com` and configure Pages to serve from it. Either works.

**2. Push this folder to that repo.**
   From the Terminal on his machine, in this directory:
   ```
   git init
   git add index.html README.md
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/<HANDLE>/<REPO>.git
   git push -u origin main
   ```
   Within ~60 seconds, the site is live at `https://<HANDLE>.github.io` (or the repo URL).

**3. Point xavierbarbouche.com at it.**
   Depends on where he bought the domain. Tell me which registrar, and I'll give you the exact DNS records. Short version for the three likely ones:

   - **Cloudflare Registrar** — DNS tab → add four `A` records for `@` pointing to GitHub's IPs (`185.199.108.153`, `.109.153`, `.110.153`, `.111.153`) and a `CNAME` for `www` → `<HANDLE>.github.io`. Orange-cloud OFF (DNS-only) for Pages to issue the certificate.
   - **Namecheap** — Advanced DNS → same four `A` records for `@`, `CNAME` for `www`.
   - **GoDaddy** — DNS Management → same four `A` records, `CNAME` for `www`. (If he bought from GoDaddy, consider transferring to Cloudflare later — cheaper, cleaner.)

   Then in the GitHub repo: **Settings → Pages → Custom domain → `xavierbarbouche.com` → Enforce HTTPS** (once DNS propagates, the toggle becomes available).

## Open questions for Xavier

1. Which registrar was used for the domain? (Determines exact DNS steps.)
2. GitHub handle? Does he already have an account from the FHG web-dev intern days, or is this new?
3. Public email — real forwarding address on `xavierbarbouche.com` or a Gmail?
4. Headshot — does he want one on the site? V1 is intentionally typography-only, no photo. Adding one is easy once he picks it.

## Iterate from here

Once the above questions are answered and the site is live, easy next adds:

- A `/projects` section or page for the sports-attendance project writeup
- A `/writing` section if he starts a short essay stream
- A `ja.html` Japanese-language version (the resume is already in 履歴書 format — site could mirror that)
- Open Graph image for link previews
- A tiny favicon

None of those are needed to launch. Launch first, improve second.
