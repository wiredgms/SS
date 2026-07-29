# ShayneSails Website Project Handoff

Date: July 29, 2026

## Project Location

Local project folder:

`/Users/wired/Documents/New project`

Local preview file:

`file:///Users/wired/Documents/New%20project/index.html#top`

## Current Site State

This is a static landing page for `www.shaynesails.com`. It does not require a build step, package install, server framework, or external CDN.

Primary files:

- `index.html`: landing page markup
- `styles.css`: all styling, responsive layout, and coin animation
- `CNAME`: contains `www.shaynesails.com` for GitHub Pages-style hosting
- `.nojekyll`: tells GitHub Pages to serve the static files directly
- `assets/shayne-sails-coin.jpeg`: original challenge coin image
- `assets/shayne-sails-coin-keyed.png`: current transparent-background hero coin asset
- `assets/shayne-sails-coin-cutout.png`: older transparent coin attempt, retained for reference
- `assets/shayne-sails-card.jpeg`: converted business card artwork

There is also an unrelated CSV in the folder:

- `claude_code_like_projects_2026-06-13.csv`

It is not used by the site.

## Business Details Used

Company:

`ShayneSails LLC`

Tagline:

`We treat your boat as if it were our own.`

Phone:

`469-939-2803`

Email:

`shaynesails@icloud.com`

Services from the business card:

- Marine Consulting
- Rigging
- Electronics
- Yacht Brokerage
- Inboard & Outboard Sales and Service

Experience statement:

`25 years of experience`

## Design Notes

The hero section uses the challenge coin image as a large visual element. It spins slowly using a flat CSS rotation so the artwork stays fully visible and does not bounce at edge-on positions.

The coin is intentionally one image element after earlier thickness/front-back/edge-on attempts looked wrong:

- `.hero-coin`: the single animated image element
- `assets/shayne-sails-coin-keyed.png`: transparent-background coin artwork

Relevant CSS:

- `.hero`: contains the visual stage
- `.hero-coin`: positions and sizes the coin
- `@keyframes coin-flat-spin`: rotates the coin with `rotate(0deg)` to `rotate(360deg)`

The coin has been moved inward so the right edge should not be cut off:

- Desktop: `right: clamp(24px, 4vw, 80px)`
- Desktop width: `min(50vw, 640px)`
- Mobile width: `min(92vw, 430px)`

The hero tagline is forced to remain on one line:

- `.hero-copy` has `white-space: nowrap`
- Desktop font size: `clamp(20px, 2.25vw, 34px)`
- Mobile font size: `clamp(17px, 4.4vw, 22px)`

## Coin Transparency Context

The current hero image is:

`assets/shayne-sails-coin-keyed.png`

It was generated from the original JPEG using Node with the `sharp` package. The approach was border-connected color-key removal:

- Start from the image border.
- Flood-fill only pixels that look like white/light neutral background or neutral gray shadow.
- Preserve warmer brass-colored pixels so the coin rim remains visible.
- Write alpha `0` for removed background pixels and `255` for retained coin pixels.

The selected thresholds were:

```js
{ avg: 135, spread: 34, warm: 14, light: 210, lightSpread: 100 }
```

This was chosen because the previous fixed ellipse/circle crop looked like a mask and either clipped the rim or preserved unwanted shadow. The keyed image should better remove the original white background around the lower edges while keeping the full coin.

If the local browser still appears to show the old asset, hard refresh or verify that `index.html` references:

```html
src="assets/shayne-sails-coin-keyed.png"
```

## Hosting Plan

Recommended hosting path:

GitHub Pages plus GoDaddy DNS.

Suggested GitHub Pages setup:

1. Create a GitHub repository, for example `shaynesails`.
2. Push this project folder to the repository.
3. In GitHub, open repository Settings > Pages.
4. Publish from the `main` branch.
5. Set custom domain to `www.shaynesails.com`.
6. Keep the `CNAME` file in the repo.

GoDaddy DNS:

- Add or update a `CNAME` record:
  - Name: `www`
  - Value: `<your-github-username>.github.io`

Optional root-domain support for `shaynesails.com`:

Add GitHub Pages `A` records for the root/apex domain:

```text
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

DNS changes can take time to propagate.

## Known Limitations

Local automated browser screenshots were blocked in the managed environment because headless browser/server launch was restricted. The page is static and can be opened directly in a browser with the `file://` URL above.

The current site is one page only. Future pages can be added later for service details, brokerage, projects, testimonials, or contact.

## Suggested Next Steps

1. Open the local `index.html` and check the coin edge, spin, hero text, and mobile sizing.
2. If the coin edge still needs cleanup, regenerate `assets/shayne-sails-coin-keyed.png` from the original JPEG with adjusted color-key thresholds.
3. Create/push to a GitHub repo.
4. Enable GitHub Pages and configure GoDaddy DNS.
5. Add service detail pages after the landing page is online.
