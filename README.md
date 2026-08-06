# Price Recon

Competitor price-reconciliation dashboard for gift cards. Tracks our prices against competitors (G2G, OffGamers, Kinguin, Seagm, G2A, Codashop, LapakGaming, Unipin, Eneba, MooGold) and flags where we're out of line.

Live dashboard: served by GitHub Pages from this repo root (`index.html`).

## Files

| File | Purpose |
|------|---------|
| `index.html` | The dashboard itself. Product catalog is embedded as `RAW_DATA`; prices load at runtime from `prices.json`. |
| `urls.json` | Product + competitor URL list read by the scraper. |
| `scrape.py` | Fetches competitor prices and writes `prices.json`. |
| `prices.json` | Latest scraped prices (auto-updated by the scrape workflow). |
| `price_history.json` | Historical price snapshots. |
| `dashboard_state.json` | Saved layout, tab order, and manual overrides. |
| `.github/workflows/` | GitHub Action that runs the scrape. |

## Adding a product

Every product must be added to **two** places (they feed different parts of the system):

1. `index.html` → `RAW_DATA[category].products` — controls what shows on the dashboard.
2. `urls.json` — controls what the scraper collects prices for.

Rules:
- Match the existing naming convention for that category.
- If a competitor doesn't sell that exact denomination, set the cell to `N/A` — never a substitute price.
- Within a category, group by currency and list denominations in descending order.

## Deploying

1. Upload changed files to the **repo root** (not a subfolder, or GitHub Pages shows a directory listing).
2. Hard-refresh the dashboard (Cmd+Shift+R) to clear the cached old `index.html`.
3. Run the "Scrape competitor prices" Action (or the dashboard's ⟳ Sync Latest button) to fill in prices for new products.
