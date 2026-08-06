# How to Add Products to Price Recon

Standard workflow whenever new products need to be added to the dashboard.

## Input

Li Zhen uploads an **Excel/CSV** of products to add. Expected columns:
`Product Name` followed by one column per competitor
(`G2G, OG, Kinguin, Seagm, G2A, Codashop, LapakGaming, Unipin, Eneba, Moogold`),
each cell holding the competitor's product URL (blank = the competitor doesn't sell it → `N/A`).

## What Claude delivers

Two updated files, ready to upload to the GitHub repo root:

1. **`urls.json`** — so the scraper collects the new products' prices.
2. **`index.html`** — so the new products display on the dashboard (catalog is the embedded `RAW_DATA`).

Both must be updated together — one feeds the scraper, the other feeds the display.

## Steps

1. Read the uploaded Excel/CSV; parse each row into a product (name + competitor URLs).
2. Normalise names to match the existing convention in that category.
3. Map the `Moogold` column to the `MooGold` key. Empty cells → `N/A`.
4. Add each product to `urls.json` (nested shape: `{name, urls:{COMP:{url, note?}}}`).
5. Add each product to `RAW_DATA` in `index.html` (flat shape: `{name, urls:{COMP:"url-or-N/A"}}`), filling every competitor key for that category.
6. **Layout:** within each category, group by currency and order denominations **descending by value**, following the existing currency sequence. New denominations slot into their currency group — not appended at the bottom.
7. Validate: `RAW_DATA` parses as JSON with the expected product count; every `<script>` block compiles with no errors.
8. Hand back the updated `urls.json` and `index.html`.

## Deploy (Li Zhen does this)

1. Upload both files to the repo **root** (github.com/lizhenong-ai/Price-Recon, `main`) — not a subfolder, or GitHub Pages shows a directory listing.
2. Hard-refresh the dashboard (Cmd+Shift+R) to clear the cached old `index.html`.
3. Run the "Scrape competitor prices" Action (or the ⟳ Sync Latest button) to fill in prices.

## Rules

- **N/A rule:** if a competitor doesn't sell that exact denomination, the cell is `N/A` — never a substitute/closest price.
- Naming must match the existing convention for the category.
