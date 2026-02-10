# Commercial Property Listings – Singapore

Static webpage to view commercial property listings on a Singapore map with a filterable, sortable list.

## Quick start

1. **View the page**  
   Because the page loads `listings.json` with `fetch`, open it over HTTP (not as `file://`):
   ```bash
   cd property-listings
   python3 -m http.server 8000
   ```
   Then open [http://localhost:8000](http://localhost:8000).

2. **Edit listings**  
   Edit `listings.json` to change addresses, prices, URLs, status, or comments. Add an optional `imageUrl` per listing to show a photo in the list and in the map popup.

3. **Add listings via CSV**  
   At the top of the page, paste a Google Sheet CSV link (File → Share → Publish to web → CSV) and click **Load**, or use **Upload CSV** to add rows from a file. CSV columns: address, url, sizeSqFt, pricePerMonth, psf, mrt, status, comments, lat, lng. You can **Remove** any listing from the list (hidden in this browser until you click **Restore removed**), or **Download current listings (JSON)** to replace `listings.json` and push so the live site updates for everyone.

## Geocoding (one-time)

The map needs `lat` and `lng` for each listing. The repo ships with placeholder coordinates. To fill real coordinates from addresses, run the geocoding script (no pip install required; it uses only Python’s standard library):

```bash
cd property-listings
python3 geocode_listings.py
```

This reads `listings.json`, geocodes each address via the Nominatim (OpenStreetMap) REST API, and writes updated `lat`/`lng` back into `listings.json`. It waits about 1 second between requests to respect Nominatim’s usage policy.

After geocoding, refresh the webpage to see markers at the correct locations.

## Publishing (make it public)

To let others view the site at a public URL, host the static files on a free service. No code changes are required.

**GitHub Pages**

1. Create a new repository on GitHub (e.g. `property-listings`).
2. Push at least `index.html` and `listings.json` to the repo (same folder so `fetch('listings.json')` works). You can include `README.md` and `geocode_listings.py`; the script is for your use only and is not served.
3. In the repo go to **Settings → Pages**. Under “Build and deployment”, set **Source** to your main branch and the root (or `/docs` if you put the files there). Save.
4. After a short delay the site is live at `https://<username>.github.io/<repo>/`. Share that URL; pushing changes to the repo updates the live site.

**Netlify (alternative)**

1. Go to [netlify.com](https://www.netlify.com) and sign up or log in.
2. **Sites → Add new site → Deploy manually**: drag the folder that contains `index.html` and `listings.json`, or connect your Git repo and set the publish directory to that folder.
3. Netlify gives a URL like `https://<name>.netlify.app`; you can change the site name in **Site settings → Domain management**. To update the site, drag the folder again or push to the connected repo.

The live site is public and can be found by search engines unless you add restrictions (e.g. Netlify password protection on a paid plan, or a `noindex` meta tag if you add one later).
