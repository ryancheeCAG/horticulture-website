# Changi Canopy — Plant Sales Website

A boutique houseplant storefront for **Changi Canopy**, inspired by the living green walls of Singapore Changi Airport. Built as a set of self-contained HTML pages (Design Components) with a shared runtime, no build step, and no backend.

---

## Highlights

- **Wonderfall hero + splash** — an intro splash (video) and a home hero where moving the cursor across the room reveals a fern & moss "living wall" through a soft spotlight mask.
- **Working shop** — filterable product grid, a persistent cart, checkout, and an A4-printable order receipt.
- **Live inventory** — stock counts shown on every product and kept in sync with an admin inventory manager.
- **Admin dashboard** — profit & loss logging plus inventory reset tools, behind a sign-in gate.
- **Customer touchpoints** — a hidden feedback sidebar and WhatsApp contact / order relay.
- **Responsive** — adapts to phones and tablets: the navigation, hero, dashboards, and product grid reflow for small screens.

---

## Pages

| File | Purpose |
|------|---------|
| `index.html` | Home. Splash intro + interactive hero, featured plants, story sections. |
| `shop.html` | Full catalogue, filters, cart drawer, checkout, printable receipt, feedback sidebar, WhatsApp. |
| `care-assistant.html` | Plant-care guidance / recommendations. |
| `pnl-dashboard.html` | Admin: Profit & Loss tracker + Inventory manager (sign-in required). |
| `terms.html` / `privacy.html` / `disclaimer.html` | Legal pages. |
| `support.js` | Shared runtime that powers all pages. **Do not edit.** |

---

## Key features in detail

### Splash & navigation
- The splash intro shows **once per browser session**. After it's dismissed (or after the first view), navigating around the site won't replay it.
- Every **"Home"** link points to `index.html?home=1`, which skips the splash and lands directly on the main hero — so returning home never re-triggers the intro.

### Interactive hero (home)
- A base video/room image with a fern & moss "living wall" layer on top, revealed by a radial spotlight that follows the cursor.
- Tweakable options are exposed on the home component:
  - **Hero wall** — `hover reveal` (default), `fern & moss` (always shown), or `wonderfall` (always base).
  - **Reveal radius** — size of the spotlight (200–1200px).
  - **Opening** — toggle the splash intro on/off.

### Shop & cart
- **Add to cart** persists to the browser (localStorage) and survives reloads.
- **Cart drawer** — adjust quantities, remove items, see a live subtotal.
- **Checkout** collects name, email, and delivery address, then produces an **order confirmation**.
- **Printable receipt** — the confirmation includes a "Print / Save PDF" action laid out for **A4** paper (browser print → Save as PDF).
- **Live stock** — each product shows "N in stock" with a status dot; the count decrements automatically when an order is placed.

### Feedback sidebar
- A hidden **"Feedback"** tab on the edge of the shop slides out a form (star rating + optional name + message). Submissions are stored locally.

### WhatsApp integration
- Dummy number: **+65 8836 5287**.
- **"Ask us"** floating button opens WhatsApp with a pre-filled question starter.
- **On checkout**, WhatsApp opens pre-filled with the full order details (order ID, customer, delivery address, line items, total), ready to send in one tap.
- Note: browsers can't send a WhatsApp message silently — the chat opens pre-filled and the sender taps send. True automatic sending requires the WhatsApp Business API + a backend.

### Admin dashboard (`pnl-dashboard.html`)
- **Sign-in gate** — username + password required; the session is remembered until sign-out. Credentials are set in the page's script (not documented here).
- **Access alert** — a successful sign-in opens WhatsApp pre-filled with an "admin dashboard was accessed" notice for the shop number.
- **Profit & Loss** — log daily revenue/costs; today/week/month totals and a 14-day chart roll up automatically.
- **Inventory** — reset stock per plant, select multiple (or all) plants and apply one quantity at once, plus "Restock all to 20" / "Set all to 0" shortcuts. Changes sync live with the shop.

### Responsive design
- Layouts adapt across desktop, tablet, and phone.
- On small screens the top navigation compacts (labels shrink, the secondary call-to-action collapses) so it never overflows.
- The home hero re-centers its caption and call-to-action, and the "move your cursor" hint (a desktop-only interaction) is hidden on touch devices.
- The shop grid drops to a single column, and the admin dashboard's P&L and inventory panels stack vertically.

---

## Data & persistence

All state is stored in the browser (localStorage / sessionStorage) — there is no server or database. Keys used:

- `cc_cart` — current cart contents
- `cc_inventory` — per-plant stock counts
- `cc_orders` — placed orders
- `cc_feedback` — submitted feedback
- `cc_seen_splash` — whether the splash has shown this session
- `cc_admin_authed` — admin sign-in state for the session

Because data is per-browser, it is not shared between devices or visitors, and clearing browser data resets everything.

---

## Running locally

No build step. Serve the folder with any static server, e.g.:

```bash
python3 -m http.server 8000
# then open http://localhost:8000/index.html
```

Opening `index.html` directly via `file://` also works, though a local server is recommended so relative links behave consistently.

---

## Tech notes

- Pages are authored as **Design Components** (`<x-dc>` markup + a `Component` logic class) and rendered by the shared `support.js` runtime. Styling is inline; fonts are **Playfair Display** (display) and **Lato** (body).
- Brand colors: deep green `#00532b`, leaf green `#008545`, accent red `#CC1000`, cream `#F6F1E7`.
- No external dependencies beyond Google Fonts and the hosted media assets referenced by URL.

---

## Limitations / not production-ready

- **No real authentication or security.** The admin gate runs entirely in the browser; do not rely on it to protect sensitive data.
- **No payments** — checkout is a demo flow; no money is processed.
- **No real order delivery** — orders live only in the local browser and the pre-filled WhatsApp message.
- Intended as a **prototype / marketing site**, not a transactional e-commerce backend.

---

© 2026 Changi Canopy. All rights reserved.
