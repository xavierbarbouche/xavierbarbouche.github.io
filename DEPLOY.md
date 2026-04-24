# Xavier: deploy `xavierbarbouche.com` — step by step

Domain: **xavierbarbouche.com** (Cloudflare Registrar — good choice).
Files in this folder: `index.html`, `README.md`. That's the whole site.
Goal by end of this sitting: a live URL at **https://xavierbarbouche.com**.

Follow top to bottom. Stop and flag anything that doesn't match what's described.

---

## Step 1 — GitHub account (5 min)

If you already have a GitHub account with a handle you're happy to use publicly, skip to Step 2.

1. Go to **github.com**, sign up.
2. For username, pick one you can live with for years. Good options, in order:
   - `xavierbarbouche` (ideal, but likely taken)
   - `xbarbouche`
   - `xavier-barbouche`
   - `xavierb-dev` (only if the others are gone)
3. Verify your email. Free plan is all you need.

**Handle confirmed:** `xavierbarbouche`. All commands below are filled in with that handle. If GitHub forced you to pick a different one, tell Michael and I'll re-issue the runbook.

---

## Step 2 — Create the repository (2 min)

1. Signed in to GitHub, click **+** (top right) → **New repository**.
2. **Repository name:** `xavierbarbouche.github.io` — exactly that, lowercase, the `.github.io` suffix is literal.
   - This is a GitHub convention: a repo named `<handle>.github.io` auto-publishes to `https://<handle>.github.io` as a personal site. Our custom domain goes on top of that later.
3. **Public.** Must be public for free GitHub Pages.
4. **Do not** check "Add a README" or "Add .gitignore." Leave the repo empty.
5. Click **Create repository.**

You'll land on a page that says "Quick setup." Keep that tab open.

---

## Step 3 — Push the site from this folder (5 min)

Open **Terminal.app** on the Mac. Paste these blocks one at a time, in the exact order shown. No substitutions needed — the handle is already wired in.

```bash
cd ~/Downloads/4614/Claude
```

```bash
git init
git add index.html README.md DEPLOY.md
git commit -m "Initial site"
git branch -M main
```

```bash
git remote add origin https://github.com/xavierbarbouche/xavierbarbouche.github.io.git
git push -u origin main
```

The push will prompt for GitHub credentials. Use a **personal access token**, not your password:
- On GitHub: profile picture → **Settings** → **Developer settings** (bottom of left sidebar) → **Personal access tokens** → **Tokens (classic)** → **Generate new token (classic)**.
- Scope: just `repo`. Expiration: 90 days is fine.
- Copy the token, paste it in Terminal when prompted for a password.

Within ~60 seconds after the push completes, the site is live at **`https://xavierbarbouche.github.io`**. Open it in a browser. You should see the site rendered.

If you see "404" for the first minute or two, that's normal — wait, refresh.

---

## Step 4 — Point `xavierbarbouche.com` at it via Cloudflare (5 min)

This is the part that matters most. Don't skip substeps.

**4a. Log in to Cloudflare, open your domain.**

1. **dash.cloudflare.com** → click **xavierbarbouche.com** in the site list.
2. Left sidebar → **DNS** → **Records**.

**4b. Clear out any auto-added parking records.**

If Cloudflare added placeholder `A` or `CNAME` records pointing at a parking page, delete them first. You want a clean slate.

**4c. Add four A records for the apex (`@`).**

Click **Add record** four times, once for each IP below. Every record uses:
- **Type:** `A`
- **Name:** `@` (or `xavierbarbouche.com` — Cloudflare auto-converts)
- **Proxy status:** **DNS only** (gray cloud, NOT orange). Important — the orange-cloud proxy breaks GitHub's ability to issue the TLS cert on first setup. We can turn it back on later if we want.
- **TTL:** Auto.

The four IPs:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**4d. Add one CNAME record for `www`.**

- **Type:** `CNAME`
- **Name:** `www`
- **Target:** `xavierbarbouche.github.io` (note: `.github.io`, no trailing path)
- **Proxy status:** **DNS only** (gray cloud).
- **TTL:** Auto.

**4e. Save.** You should now see five records in the DNS tab: four `A` records for `@`, one `CNAME` for `www`.

---

## Step 5 — Tell GitHub about the custom domain (2 min)

Back on GitHub, in the `xavierbarbouche.github.io` repo:

1. **Settings** (top of repo) → **Pages** (left sidebar).
2. Under **Custom domain**, enter `xavierbarbouche.com` → **Save**.
3. GitHub will run a DNS check. It may take a few minutes — refresh the page every minute or so.
4. Once the check passes, **Enforce HTTPS** becomes available. Check it. This issues the Let's Encrypt cert automatically.

GitHub will also add a `CNAME` file to your repo. That's expected — let it.

---

## Step 6 — Visit the site (1 min)

Open **https://xavierbarbouche.com** in a fresh tab. Should render identically to `https://xavierbarbouche.github.io`, but at the real domain, with the lock icon showing valid HTTPS.

If HTTPS fails the first time (`ERR_SSL_` or similar), wait 10 minutes and try again. The first cert issuance can lag DNS propagation. If it's still failing after an hour, tell Michael and I'll debug.

---

## Step 7 — Easy edits going forward

Any time Xavier wants to change the site:

1. Edit `index.html` in this folder (any text editor — VS Code ideal).
2. In Terminal:
   ```bash
   cd ~/Downloads/4614/Claude
   git add -A
   git commit -m "describe the change"
   git push
   ```
3. Site updates within ~60 seconds.

That's the whole loop.

---

## If something goes wrong

Copy the error text (or take a screenshot), send it to Michael, and he'll paste it to me. I'll walk through it step by step.

The most common hang points:
- **Push rejected** — probably authenticating with password instead of personal access token.
- **404 on `xavierbarbouche.com`** — DNS hasn't propagated yet, or `A` records have the wrong IPs, or the records are proxied (orange cloud). Check Cloudflare: all five records must be **gray cloud**.
- **Certificate error** — wait longer, then re-check "Enforce HTTPS" in GitHub Pages settings.
