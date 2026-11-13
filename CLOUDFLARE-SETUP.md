# A$AD Protocol: Shield update + Cloudflare Pages setup

## What's in this update

| File | What it does | Where it goes |
|---|---|---|
| `shield.html` | The new **Shield** section: 35 Manipulation Tactics + 94 Dark Conversation Hacks, with search, ☆ saved cards and ⚡ Drill mode. Self-contained (data is built in). | Repo root, next to `index.html` |
| `playbook-data.json` | The same 129 tactics as plain data, in case you want to use them elsewhere in the app later. | Repo root (optional) |
| `_headers` | Tells Cloudflare not to cache the HTML, so your phone gets each update straight away. Also hides the site from Google. | Repo root |
| `CLOUDFLARE-SETUP.md` | This guide. | Repo root (optional) |

## Step 1: Add a button to the app's home screen

Open `index.html` in the repo and paste this wherever your other menu buttons or links are:

```html
<a href="shield.html" style="display:inline-block;padding:12px 18px;border:1px solid #FF4FA3;border-radius:12px;color:#FF7DB8;text-decoration:none;font-weight:600">🛡️ Shield: Know the Moves</a>
```

The Shield page has its own "← App" button that takes you back to `index.html`.

## Step 2: Upload the files to GitHub

**From a computer (easiest):**
1. Go to `github.com/<your-username>/asad-protocol`.
2. Click **Add file → Upload files**.
3. Drag in `shield.html`, `_headers` and `playbook-data.json` (plus your edited `index.html`).
4. Commit message: `Add Shield section + Cloudflare headers`. Leave **Commit directly to the main branch** selected and click **Commit changes**.

**From the command line:**
```bash
git clone https://github.com/<your-username>/asad-protocol.git
cd asad-protocol
# copy the files in, then:
git add shield.html _headers playbook-data.json index.html
git commit -m "Add Shield section + Cloudflare headers"
git push origin main
```

> `_headers` starts with an underscore and has **no file extension**. If Windows renames it to `_headers.txt`, rename it back.

## Step 3: Connect the repo to Cloudflare Pages (first time only)

Skip to Step 4 if the project is already connected and deploying.

1. Log in at **dash.cloudflare.com**.
2. Go to **Workers & Pages → Create application**.
3. Choose the **Pages** tab, *not* Workers.
4. Click **Import an existing Git repository**. If it asks, authorize **GitHub** and give the "Cloudflare Workers and Pages" app access to `asad-protocol`.
5. Pick `asad-protocol` and click **Begin setup**.
6. Fill in **Set up builds and deployments**:
   - **Project name:** `asad-protocol` (your URL becomes `asad-protocol.pages.dev`)
   - **Production branch:** `main`
   - **Framework preset:** `None`
   - **Build command:** `exit 0`
   - **Build output directory:** `/` (use the folder name instead if `index.html` sits in a subfolder such as `public`)
7. Click **Save and Deploy**. It takes about a minute, then gives you the live URL.

## Step 4: If the project is already connected but updates don't show

Go to **Workers & Pages → asad-protocol** and check each of these:

1. **Deployments tab:** your latest commit should appear within a minute of pushing. If it shows **Failed**, open it and read the build log.
2. **Settings → Builds:** the production branch must be `main` (or whatever branch you push to), and **Automatic deployments** must be on.
3. **Build output directory** must be the folder that contains `index.html`. A wrong value here causes a 404 or a blank site.
4. **GitHub permissions:** on GitHub go to **Settings → Applications → Installed GitHub Apps → Cloudflare Workers and Pages → Configure**. Make sure `asad-protocol` is in the allowed repositories. If it isn't, Cloudflare never hears about your pushes.
5. To deploy straight away, open **Deployments**, click **⋯** on the latest one and choose **Retry deployment**.

## Step 5: Get the update on your phone

- Open the site in Chrome and pull down to refresh.
- If you installed it as an app (home-screen icon) and it still shows the old version, close it fully and reopen it. If that doesn't work, go to **Chrome → Settings → Site settings → All sites → asad-protocol.pages.dev → Clear & reset**, then reopen it. Your PIN and saved Shield cards are stored on the phone, so clearing site data resets them.
- If your app has a `service-worker.js` or `sw.js`, change the version or cache name inside it (e.g. `v3` → `v4`) whenever you update. Otherwise the old version can stay cached.

## Step 6 (recommended): Lock the site behind a real login

The app's PIN runs in the browser, so anyone who knows the URL can still load the pages. To make the site truly private for free:

1. In the Cloudflare dashboard open **Zero Trust**. The free plan is fine.
2. Go to **Access → Applications → Add an application → Self-hosted**.
3. Domain: `asad-protocol.pages.dev`. Policy: **Allow**, Include → **Emails** → your email.
4. Save. Visitors now have to enter a one-time code sent to your email before the site loads, and your PIN still works on top of that.
