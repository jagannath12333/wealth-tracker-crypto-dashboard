# Frontend Assignment — Brito Jaison

Completed **both** tasks as standalone single-file HTML apps. No build step required.

---

## Task 1 — Personal Wealth & Expense Tracker (`wealth-tracker.html`)

### Live Demo
> Deploy `wealth-tracker.html` to Netlify Drop or GitHub Pages.

### Tech Stack

| Library / Tool | Why |
|---|---|
| **Vanilla JS (ES6+)** | Zero framework overhead for a client-side-only app. Faster load, no build step. |
| **Chart.js 4.4** | Mature, well-documented charting library. Doughnut + Bar both supported with a single API. |
| **LocalStorage** | Simple key-value persistence that survives refreshes. No server needed. |
| **Google Fonts (Syne + Outfit + DM Mono)** | Distinct typographic hierarchy — geometric display, clean body, monospace numbers. |

### Features
- Add transactions with Title, Amount, Category, Type (Income / Expense), Date, Note
- Three summary cards: Total Income, Total Expenses, Net Balance (with avg per transaction + savings rate)
- **Doughnut chart** with toggle to **Bar chart** — updates instantly on every change
- Transaction history as a sortable table with inline **Edit** (modal) and **Delete**
- Empty state message on first load
- Search + Type filter + Category filter on history
- Toast notifications for all actions

### Precision & Edge Cases
- All amounts formatted with `en-IN` locale: `₹1,23,456.00`
- Large values auto-compact: `₹12.50 L`, `₹1.25 Cr`
- Cap at ₹10 Trillion — UI never breaks on huge inputs
- Decimal precision: `Math.round(amt * 100) / 100` — no floating-point drift

### Setup (Local)
```bash
# No install required. Just open the file:
open wealth-tracker.html
# or double-click in your file explorer
```

### Trade-offs & What I'd Improve
- **IndexedDB instead of LocalStorage** — for larger datasets with better async performance
- **Date range filter** — filter transactions by week / month / custom range
- **Export to CSV** — users should be able to download their data
- **Multi-currency support** — currently INR only
- **No unit tests** — would add Jest tests for the Fmt utility and calculation logic

---

## Task 2 — Real-Time Crypto & Market Pulse Dashboard (`crypto-dashboard.html`)

### Live Demo
> Deploy `crypto-dashboard.html` to Netlify Drop or GitHub Pages.

### Tech Stack

| Library / Tool | Why |
|---|---|
| **Vanilla JS (ES6+)** | Lightweight — no React/Vue bundle size for a single HTML file |
| **CoinGecko API (free tier)** | No API key required, returns price, 24h change, market cap, high/low, sparkline |
| **Custom Store (Redux pattern)** | Centralized state with `dispatch` / `subscribe` — mimics Redux without the dependency |
| **Canvas API** | Hand-drawn sparklines — no chart library needed for the 7-day mini charts |
| **LocalStorage** | Watchlist + theme persistence |
| **Google Fonts (Syne + Outfit + DM Mono)** | Same design system as Task 1 for cohesion |

### Features
- **Top 10 cryptos** by market cap — live from CoinGecko
- **Ticker strip** with animated horizontal scroll
- **4 stat cards** — total market cap, 24h volume, gainers/losers, watchlist count
- **Search** by name or symbol (Ctrl+K shortcut)
- **4 filter tabs** — All, Watchlist, Gainers, Losers
- **Column sort** — click any column header
- **Sparkline** (7-day mini chart) per coin, drawn on Canvas
- **Star / watchlist** — persistent across refreshes
- **Detail modal** — 24h high/low, market cap, supply, ATH, price range progress bar
- **Refresh button** — no page reload, with loading spinner
- **Auto-refresh** every 60 seconds
- **Dark / Light mode** toggle — persisted in LocalStorage
- **Offline detection** — banner + graceful fallback to 5-minute cache

### Error Handling
| Scenario | Behaviour |
|---|---|
| API rate limit (429) | Human-readable error + fallback to cached data |
| Network offline | Offline banner + cached data shown |
| No cache + offline | Empty state with retry button |
| API 5xx | Error message with retry link |

### Performance
- **Cache-first strategy** — loads cached data instantly, then fetches fresh in background
- **No duplicate calls** — single fetch per refresh cycle
- **Skeleton loading** — perceived performance while data loads
- **`requestAnimationFrame`** for sparkline rendering — never blocks main thread

### Code Cleanliness
- `CryptoService` — all API calls in one isolated object
- `Store` — all state mutations go through `dispatch(action)`, UI reads via `subscribe`
- `Fmt` — all formatting helpers in one utility object
- `renderTable`, `renderStats`, `renderTicker` — separate pure render functions

### Setup (Local)
```bash
open crypto-dashboard.html
# Internet connection required for live data
# Works offline with cached data (5-minute cache)
```

### Trade-offs & What I'd Improve
- **React + Context API** — for a real production app, component-based architecture scales better
- **WebSocket / SSE** — for true real-time streaming instead of polling
- **More coins** — paginate beyond top 10
- **Portfolio tracker** — let users enter holdings and track value
- **Price alerts** — browser notifications when a coin crosses a threshold
- **Historical chart** — Chart.js line chart for 7d/30d/1y price history in the modal

---

## Project Structure

```
/
├── wealth-tracker.html     # Task 1 — single file, no dependencies
├── crypto-dashboard.html   # Task 2 — single file, fetches CoinGecko API
└── README.md
```

## Running Locally

Both files are **zero-dependency** single HTML files.

1. Clone or download the repository
2. Open either file directly in a browser — no `npm install`, no build step
3. For the crypto dashboard, ensure you have an internet connection on first load

## Contact

Assignment submitted for evaluation.  
Recruiter contact: Brito Jaison — 6238260864
