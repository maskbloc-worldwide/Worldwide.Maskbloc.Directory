# Maskbloc.WorldWide.Directory
A directory of all known and active Mask Blocs, Clean Air Orgs, and other Covid-19 focused Mutual Aid Groups

A free, static, low-maintenance directory of Mask Blocs worldwide — with search, a region-grouped listing, a self-hosted interactive map (Leaflet + OpenStreetMap), and a public submission form that routes through **human review** before anything goes live.

Three files, no database, no server required:

```
index.html    the whole site (HTML + CSS + JS in one file)
blocs.json    the data 
README.md     this file
```

The directory and the map both read from `blocs.json`. To publish an approved bloc, you add one entry to that file and commit. That's the entire content workflow.

---

## 1. Put it online (free)

**GitHub Pages (recommended — pairs perfectly with the approval workflow below)**
1. Create a new GitHub repository, e.g. `maskbloc.worldwide`.
2. Upload `index.html`, `blocs.json`, and `README.md`.
3. Repo **Settings → Pages → Source: Deploy from a branch → `main` / root → Save**.
4. Your site is live at `https://maskbloc-htx.github.io/maskbloc.worldwide/` in a minute or two.

---

## 2. Turn on submissions (Formspree — free, no backend)

The form works out of the box in a fallback mode (it opens the submitter's email app). To collect submissions properly and get an email notification each time:

1. Sign up at **https://formspree.io** (free tier covers ~50 submissions/month).
2. Create a form; copy its endpoint, e.g. `https://formspree.io/f/abcdwxyz`.
3. In `index.html`, find the **Config** block near the bottom and set:
   ```js
   const FORMSPREE_ENDPOINT = "https://formspree.io/f/abcdwxyz";
   const CONTACT_EMAIL = "you@example.com"; // fallback only
   ```
4. Commit. Done — submissions now email you, with built-in spam filtering (plus the hidden honeypot field already in the form).

Alternatives if you outgrow Formspree: **Tally**, **Basin**, or **Google Forms** (point the form's `action` there instead).

---

## 3. The human-review → publish workflow

This is the "automated with human verification" part. A static site can't write to its own data file when you click approve (that needs a server), so the realistic low-maintenance loop is:

```
Someone submits the form
        │
        ▼
You get an email from Formspree with all the fields
        │
        ▼
You read it. Legit? ──No──► ignore / reply
        │
       Yes
        │
        ▼
Add one entry to blocs.json and commit ──► live on site + map within ~1 min
```

### Making "add one entry" nearly one-click

Each approved bloc is a JSON object. Copy this template, fill it from the email, paste it into the `blocs` array in `blocs.json` (mind the commas), and commit:

```json
{
  "name": "Mask Bloc Springfield",
  "region": "USA – Midwest",
  "location": "IL (Illinois)",
  "description": "Free N95s and rapid tests. DM to request.",
  "status": "active",
  "links": [
    { "label": "Instagram", "url": "https://instagram.com/example" },
    { "label": "Request Form", "url": "https://forms.gle/example" }
  ],
  "lat": 39.7817,
  "lng": -89.6501
}
```

On GitHub you can do this entirely in the browser: open `blocs.json` → pencil icon → paste → **Commit**. GitHub Pages redeploys automatically. That's your "approve" button.

**`status` values** (control the colored badge): `active`, `getting-started`, `inactive`, `clean-air`, `care-collective`, `advocacy`, `campus`, `charity`.

**Coordinates:** right-click a spot in Google Maps → click the lat/lng at the top to copy. Omit `lat`/`lng` if unknown — the bloc still lists, it just won't pin on the map.

### Want true one-click auto-publish later?

Keep everything above and add a small **serverless function** (Cloudflare Workers or Netlify Functions, both free) that receives the submission and opens a GitHub pull request via the API. Your approval becomes "merge PR." The data shape doesn't change, so it's a clean drop-in upgrade — nothing else needs rewriting.

---

## 4. The map

The map is now **self-hosted** using [Leaflet](https://leafletjs.com) with [OpenStreetMap](https://www.openstreetmap.org) tiles — no Google account, no separate map to maintain, and no API key. It reads directly from `blocs.json`: **any entry with `lat` and `lng` is plotted automatically.** When you approve a bloc by adding it to the data file, its pin appears on the next page load. Nothing else to do.

Entries without coordinates still appear in the directory list — they just won't have a pin. To add one later, edit the entry and fill in `lat`/`lng`.

**Getting coordinates:** right-click a location in Google Maps (or [openstreetmap.org](https://www.openstreetmap.org)) and copy the two numbers — latitude first, then longitude.

**Notes**
- Leaflet loads from a CDN (unpkg). It's pinned to a specific version with integrity hashes for safety. If you'd rather not depend on a CDN, download `leaflet.js` and `leaflet.css` into a `vendor/leaflet/` folder and point the two `<head>` tags at the local copies.
- OSM tiles are free for reasonable community use. If the site ever gets very high traffic, swap the tile URL for a free-tier provider (e.g. Carto, Stadia) — it's a one-line change in the `L.tileLayer(...)` call.
- The map starts with scroll-zoom disabled so it doesn't grab the page scroll; clicking the map enables zoom. Pinch-zoom works on touch devices.

---

## 5. Editing & removing blocs

- **Edit:** change the entry in `blocs.json` and commit.
- **Remove:** delete the entry (and remove the matching pin from your My Map).
- **Honor removal requests promptly** — these are real groups, and some prefer not to be listed publicly.

---

## A note on safety & trust

Listings are public and submitted by anyone, so the human-review step matters: confirm a group is real (an active social account or site usually suffices) before publishing, and don't post private home addresses — use a city/area and the group's own contact links. This is mutual-aid infrastructure; keeping it accurate and safe *is* the work.

We keep us safe. ♥
