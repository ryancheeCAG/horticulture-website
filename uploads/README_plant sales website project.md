# Changi Canopy — Plant Sales Website

## Project status
Multi-page plant e-commerce site, built as Design Components (`.dc.html`) in this tool. A static `export/` folder (plain `.html` + `support.js`) is what actually gets pushed to GitHub/Vercel — always regenerate it after editing any `.dc.html` source (see "Export workflow" below).

## Brand
- Name: **Changi Canopy** (renamed from an earlier placeholder "Rootwild")
- Palette (extracted from a Changi Airport brand-analysis JSON the user provided):
  - Primary green: `#008545` (was forest green `#2f4a3a`)
  - Dark green (footers/nav/splash bg): `#00532b` (was `#1c2b20`)
  - Accent red: `#CC1000` (was terracotta `#c9682f`)
  - Purple accent (decorative, splash sprout stem): `#7A35B0`
  - Background: `#FFFFFF` (was linen `#f6f1e7`); alt section bg `#F7F7F7` (was `#efe7d8`)
  - Text: `#222222` primary, `#555555` secondary, `#888888` tertiary
  - Footer text on dark green: `#eaf5ee` (links), `#9fd6b8` (muted copyright)
- Fonts: **Lato** (body, replaced Inter) + **Playfair Display** italic/serif (display headings — kept from the original direction, not part of the Changi palette pull)

## Pages (all `.dc.html`, all in project root)
- **Home.dc.html** → `index.html` — splash (CSS sprout-growth animation, ~3s) → hero section with a cursor-following spotlight-reveal effect (day video underneath, night video revealed in a soft circle that follows the mouse, canvas-generated radial-gradient mask, current radius 624px, smoothed via requestAnimationFrame lerp) → intro → featured products → "Our Story" lifestyle section → care-assistant CTA → footer.
- **Shop.dc.html** → `shop.html` — product grid, filterable by light level (all/low/medium/bright), "add to cart" with a toast confirmation. 10 products including sunflower, orchid, and two Chinese New Year plants (kumquat tree, lucky bamboo).
- **Care Assistant.dc.html** → `care-assistant.html` — pick a room/setting (living room, home office, bedroom, festive) → get 3 plant recommendations + care tips. Setting cards show a short ambient looping video on hover (subtle light/leaf motion) instead of a static photo.
- **PnL Dashboard.dc.html** → `pnl-dashboard.html` — admin-only P&L tool (not linked from public nav): log revenue/costs by date, auto-rolled-up daily/weekly/monthly totals, 14-day bar chart.
- **Terms.dc.html**, **Privacy.dc.html**, **Disclaimer.dc.html** → `terms.html` / `privacy.html` / `disclaimer.html` — short, plain-language legal pages written fresh for this plant business (not copied from any real company's legal text — a scraped Changi Airport JSON did not contain usable legal copy, only search-result metadata).

## Known technical gotchas (read before editing)
1. **Mask/data-URL trap**: never pass a canvas `toDataURL()` PNG through a template `style="{{ }}"` hole — the template engine's style-string parser splits on `;`, and `data:image/png;base64,...` gets truncated at the first semicolon. Fix used throughout: set `element.style.maskImage` etc. **imperatively** via a ref callback, never through a hole.
2. The spotlight-reveal hero now uses **two `<video>` elements** (day/office/bedroom/festive ambient loops from Higgsfield `seedance_2_0`), not static images. The CSS mask is applied to the wrapping `<div>`, which clips the video the same way it clipped a background-image.
3. `Home.dc.html`'s reveal radius has been bumped twice per user request: 260px → 390px → **624px** (current).
4. Scroll-triggered entrance animations (`reveal-up`/`reveal-pop`/`reveal-left`/`reveal-right` classes + an `IntersectionObserver` per page) were added across Home, Shop, and Care Assistant — borrowed technique from a "CozyPaws" reference prompt the user pasted, not the CozyPaws branding itself.

## Export workflow (IMPORTANT — do this after any `.dc.html` edit)
The `export/` folder is what's actually deployed. It is NOT auto-synced from the `.dc.html` sources. After editing any page, re-run something like:

```js
// run_script
const renameMap = [
  ['href="Home.dc.html"', 'href="index.html"'],
  ['href="Shop.dc.html"', 'href="shop.html"'],
  ['href="Care Assistant.dc.html"', 'href="care-assistant.html"'],
  ['href="PnL Dashboard.dc.html"', 'href="pnl-dashboard.html"'],
  ['href="Terms.dc.html"', 'href="terms.html"'],
  ['href="Privacy.dc.html"', 'href="privacy.html"'],
  ['href="Disclaimer.dc.html"', 'href="disclaimer.html"'],
];
const files = [
  ['Home.dc.html', 'export/index.html'],
  ['Shop.dc.html', 'export/shop.html'],
  ['Care Assistant.dc.html', 'export/care-assistant.html'],
  ['PnL Dashboard.dc.html', 'export/pnl-dashboard.html'],
  ['Terms.dc.html', 'export/terms.html'],
  ['Privacy.dc.html', 'export/privacy.html'],
  ['Disclaimer.dc.html', 'export/disclaimer.html'],
];
for (const [src, dest] of files) {
  let content = await readFile(src);
  for (const [find, repl] of renameMap) content = replaceText(content, find, repl);
  await saveFile(dest, content);
}
```

`export/support.js` is a copy of the project-root `support.js` runtime and does not need re-copying unless that root file changes.

## Deployment (already done once)
- GitHub repo: `ryancheeCAG/horticulture-website` (connected, pushed to `main`)
- Vercel project: imported from that repo, **no build command / no framework preset** (pure static site) — deploys automatically on push.
- Live URL: `https://horticulture-website.vercel.app/`
- To update the live site: download the `export/` folder as a zip (via the download-card tool), unzip over the cloned repo, `git add . && git commit -m "..." && git push`. Vercel auto-redeploys on push to `main`.

## Outstanding / possible next steps
- User has ~700 Higgsfield credits (as of this session) partially spent on: 4 room-setting ambient videos + 2 hero ambient videos (~22.5 credits each) + several product/lifestyle images. Remaining ideas not yet done: order-confirmation illustrative moment, more product photography, upscaling hero footage.
- No real cart/checkout flow exists yet — Shop's "cart" is just a counter + toast; there's no cart drawer or checkout page.
- `suppzer0bites-main` (an F&B reference project with a calorie-calculator pattern) was reviewed for inspiration on the P&L admin tool's structure (revenue/cost/profit breakdown) but no code was copied — it's a different framework/build (has its own Node server, MongoDB models, etc.) not compatible with this DC environment.
- The `lithos_spotlight_reveal` GitHub repo (referenced early on) only ever contained a text spec, not real source — the spotlight-reveal hero here was built directly from that spec, not copied code.

## Asset credits
All photography/video on the site is AI-generated via Higgsfield (`nano_banana_pro` for images, `seedance_2_0` for ambient video loops), stored on Higgsfield's CDN (`d8j0ntlcm91z4.cloudfront.net/...`) — not hosted in this project, just linked by URL.
