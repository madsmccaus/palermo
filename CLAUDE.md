# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static single-page website presenting an essay on the Capuchin Catacombs mummies of Palermo, paired with cycling photographs by Jesse Fernandez and a live Bitcoin price ticker.

## Architecture

**No build system.** The entire site is a single `index.html` file with inline CSS and JS. Open it directly in a browser — no server, bundler, or package manager required.

### Runtime Dependencies (loaded via CDN)

- **React 18.3.1 + ReactDOM** — used only for the design tweaks panel, loaded from unpkg
- **Babel Standalone 7.29** — in-browser JSX transpilation for the tweaks panel
- **Google Fonts** — "Jacquard 12" for headings and the price display

### Key Behaviors

- **Photo cycling**: A self-invoking function rotates through 19 AVIF images (`images/p01.avif`–`p19.avif`) on each page load, persisting the index in `localStorage` under key `mummyPhotoIndex`.
- **Bitcoin price**: Fetched every 3 seconds from Coinbase spot API with CoinGecko as fallback. Displayed in the `.amount` element via `Intl.NumberFormat`. The element is selected by `data-comment-anchor="298a542afa-div"`.
- **Tweaks panel**: `index.html` references `tweaks-panel.jsx` (not present in repo) which provides `useTweaks`, `TweaksPanel`, `TweakSection`, and `TweakSlider` components. The panel controls CSS custom properties (`--photo-size`, `--price-gap`, `--right-width`) with defaults baked inline in a `TWEAK_DEFAULTS` block.

### Layout

Two-column layout at `max-width: 1440px`:
- **Left column**: photo frame (fixed height via `--photo-size`) + price display
- **Right column**: scrollable essay text (hidden scrollbar), width set by `--right-width`
- **Mobile** (≤820px): collapses to single column with fluid typography via `clamp()`

### File Structure

- `index.html` — the entire application
- `images/` — 19 AVIF photographs (`p01`–`p19`) + one PNG
- `uploads/` — source assets: original AVIF images with different naming, the essay in Markdown, and reference PNGs
- `Palermo/` — contains only a `.thumbnail` preview
- `Palermo.zip` — archive of original assets

## Development

Open `index.html` in a browser. No install or build step. To serve locally (needed if `tweaks-panel.jsx` is restored or for CORS-free font loading):

```
python3 -m http.server 8000
```

CSS custom properties to adjust layout without editing styles: `--photo-size`, `--price-gap`, `--right-width`.
