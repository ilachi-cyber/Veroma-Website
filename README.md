# Veroma Media — Deploy Package

Six-page static website. Hand-coded HTML, CSS and JavaScript. No build step, no dependencies, no framework. Drop the folder on any static host and it works.

## Files in this folder

| file | purpose |
|---|---|
| `index.html` | Homepage (the entry point, with portal door intro) |
| `impact.html` | Selected work — six client case studies with live previews |
| `system.html` | The Veroma operating system explainer |
| `assistance.html` | Twelve service detail blocks |
| `journal.html` | Editorial journal with featured essay + archive grid |
| `journey.html` | About the studio: who, story, leadership, three offices with live analog clocks |
| `README.md` | This file |

## Quick test locally (optional)

Before deploying, open `index.html` directly in your browser by double-clicking it. The whole site works from a local file:// path. Click through all the nav links to make sure every page loads.

A few things look different locally vs deployed:
- Custom fonts (Fraunces, Inter Tight, JetBrains Mono) load over the network from Google Fonts — make sure you're online.
- Live website previews on the Impact page and homepage are fetched from thum.io (a third-party screenshot service) — they take 5–15 seconds on first load, then cache for 24 hours.
- Unsplash hero images load over the network too. The dark gradient fallback shows immediately while they load.

---

## Deploy path 1 — Vercel (recommended, free, ~20 minutes)

### Step 1. Create a Vercel account
Go to **vercel.com** → Sign Up. Use your existing GitHub, Google, or email. Free plan is sufficient — it covers agency-scale traffic.

### Step 2. Deploy the folder
On the Vercel dashboard:
1. Click **Add New** → **Project**
2. Choose **"Browse all templates"** or scroll to **"Import Third-Party Git Repository"**
3. Actually the fastest path: scroll down to the **"Deploy a Template"** section and look for **"Try out Vercel with a deploy of one of our templates"** — skip that
4. The simplest method: **drag and drop**. On the Vercel dashboard, click the **Add New** dropdown → **Project** → look for the **deploy** option at the top — Vercel has been changing this UI

If drag-and-drop isn't visible, do this instead:
1. Install GitHub Desktop or use the GitHub website
2. Create a new public or private repository called `veroma-site`
3. Upload these six HTML files and the README into that repo
4. On Vercel → **Add New** → **Project** → **Import Git Repository** → pick `veroma-site` → click **Deploy**

Vercel detects the static files automatically. No build command needed. Deploy takes about 30 seconds.

### Step 3. Test the temporary URL
Vercel gives you a free URL like `veroma-site.vercel.app`. Open it in your phone and laptop. Click every nav link, every case study, every service block, every essay. Make sure:
- The portal door opens cleanly with "VeRoMa Media" visible
- The hero video plays slowly and stays vibrant
- The V cursor follows your mouse on the laptop
- All six client website previews appear
- The three analog clocks on the Journey page show the correct times
- The mobile menu opens on your phone (≤900px viewport)

### Step 4. Connect your custom domain
On your Vercel project:
1. Go to **Settings** → **Domains**
2. Add `veromamedia.com` (or whatever your real domain is) and the `www.` version too
3. Vercel shows you exactly which DNS records to set — usually:
   - An `A` record with value `76.76.21.21` (Vercel's IP)
   - A `CNAME` record on `www` pointing to `cname.vercel-dns.com`

These exact values come from Vercel — copy them from the page.

### Step 5. Change DNS at your domain registrar
This is where your domain currently points to Wix. To switch, log into wherever you bought your domain:

- **If bought through Wix:** Wix dashboard → **Domains** → click your domain → **Advanced** or **DNS Records** → find the existing `A` and `CNAME` records → replace their values with the ones Vercel gave you.
- **If bought through GoDaddy, Namecheap, BigRock, etc.:** Log in there → DNS settings for your domain → replace `A` and `CNAME` records with Vercel's values.

### Step 6. Wait for propagation
DNS changes take anywhere from 10 minutes to 24 hours to propagate globally. During this window, some visitors will see the old Wix site, some will see the new Vercel site. That's normal. Once propagated, everyone sees the new site.

### Step 7. Watch the new site live
Keep the Wix site running and paid for at least 30 days as a safety net. If anything breaks, you can flip the DNS back to Wix in under 10 minutes.

---

## Deploy path 2 — Netlify (alternative, also free)

Identical to Vercel in workflow:

1. Go to **netlify.com** → Sign up
2. On the dashboard, find the **"Sites"** section → look for the **"Want to deploy a new site without connecting to Git?"** prompt → **"Drag and drop your site output folder here"**
3. Drag the entire `veroma-deploy` folder onto that target
4. Netlify deploys it in ~30 seconds and gives you a free `*.netlify.app` URL
5. Test, then add a custom domain under **Site settings** → **Domain management**
6. Same DNS steps as Vercel — Netlify gives you the exact records to set

Use Vercel or Netlify based on whichever signup feels faster. There's no meaningful difference for a six-page static site.

---

## Important — the domain handover sequence

Do these steps **in this order** to avoid downtime:

1. ✅ Deploy to Vercel/Netlify. Get the free `*.vercel.app` or `*.netlify.app` URL.
2. ✅ Test the temporary URL on phone and laptop. Click everything.
3. ✅ Add the custom domain in Vercel/Netlify settings. They'll show "DNS pending" until step 4.
4. ✅ Only **after** you're confident the new site works on the temp URL — change the DNS records at Wix (or wherever you bought the domain).
5. ✅ Wait 1 to 24 hours for DNS propagation.
6. ✅ Confirm `veromamedia.com` now shows the new site on multiple devices.
7. ✅ Don't cancel Wix immediately — keep it as a fallback for 30 days.

---

## What about email?

If you currently use `hello@veromamedia.com` through Wix:

- Your email is handled by **MX records** in DNS, which are separate from the `A`/`CNAME` records you're changing.
- Do **not** change MX records when you switch to Vercel/Netlify. Leave them pointing to Wix's mail servers.
- Your email will keep working through Wix Mail uninterrupted.

If you also want to move email off Wix, that's a separate migration — Google Workspace and Zoho Mail are common alternatives. Don't do that in the same step as the website cutover. Move the site first, then handle email later.

---

## After you're live — production polish

The site is fully functional today, but two things are worth upgrading once you're live:

### 1. Replace thum.io screenshots with your own
The six case-study browser frames currently use `thum.io` (free screenshot service). First-time visitors wait 5–15 seconds for each preview to render, then it's cached. For peak polish:

1. Take a screenshot of each client website yourself (Rocket Reels, Parishi Capital, Cinekorn, Booster Water, Nest Developers, Gayatri AI) at 1600px wide.
2. Save them as JPGs (around 200KB each).
3. Drop them into a new `screenshots/` folder alongside the HTML files.
4. Search-and-replace the `https://image.thum.io/get/width/1600/wait/10/maxAge/24/...` URLs in `index.html` and `impact.html` with `screenshots/rocket-reels.jpg`, `screenshots/parishi.jpg`, etc.
5. Re-deploy.

### 2. Replace Unsplash hero banners with your own photography
Hero banners on Impact, System, Assistance, Journal, and Journey use Unsplash keyword queries — they return a random matching image each time. For brand consistency:

1. Pick five hero images (yours, licensed stock, or commissioned).
2. Save as JPGs at 1800px wide, around 300KB each.
3. Drop into `hero/` folder.
4. Search-and-replace `https://source.unsplash.com/1800x1000/?...` URLs with your local paths.
5. Re-deploy.

### 3. Optional — clean URLs
Vercel and Netlify both support clean URLs without the `.html` extension. To enable, add a file called `vercel.json` (for Vercel) or `_redirects` (for Netlify) with the right config — ask whoever maintains the site to set this up. It's a minor polish; not urgent.

---

## Caching note — important

Wix, browsers, and CDNs all cache aggressively. After deploying any update, **hard-refresh** to see the latest:

- **macOS Chrome/Safari:** Cmd + Shift + R
- **Windows Chrome/Firefox:** Ctrl + Shift + R
- **iPhone Safari:** open the page in a new private tab, or settings → clear history
- **Android Chrome:** menu → settings → privacy → clear browsing data

If you redeploy a fix and your phone still shows the old version, that's cache — not a broken deployment. Always hard-refresh first before assuming something's wrong.

---

## Need help?

If something doesn't work after deploying, the most common issues are:

1. **Cache.** Hard-refresh first. Then try incognito/private mode.
2. **Wrong DNS records.** Double-check the `A` and `CNAME` values match exactly what Vercel/Netlify showed you.
3. **DNS not propagated yet.** Wait 24 hours. Check propagation status at `dnschecker.org`.
4. **Email broke.** You changed MX records by mistake. Revert MX to Wix's values.

If you hit something else, take a screenshot of the problem plus the URL bar and bring it back here.
