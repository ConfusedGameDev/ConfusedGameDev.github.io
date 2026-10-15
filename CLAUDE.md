# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Jorge Pedrero's ("ConfusedGameDev") personal site, served by GitHub Pages from `main` at https://confusedgamedev.github.io. It is a static site with no build step, package manager, linter or tests: whatever is committed is what gets served. Pushing to `main` deploys.

To preview locally, serve the repo root over HTTP (e.g. `python3 -m http.server 8000`). Opening files via `file://` breaks the Unity WebGL builds.

`_config.yml` sets a Jekyll theme (`jekyll-theme-cayman`), so GitHub Pages runs the repo through Jekyll. The HTML pages have no front matter, so Jekyll copies them through as-is. Files or folders whose names start with `_` or `.` are not published.

## Layout

The repo holds several independent sites under one domain. Nothing is shared between them.

- **Portfolio (root)**: `index.html` is a single self-contained page: all CSS lives in its one `<style>` block and all behavior in its one `<script>` block at the end. There are no vendor libraries, no build step and no external dependencies apart from Google Fonts. Sections are `main > section[id]` (`#work`, `#about`, `#skills`, `#resume`, `#contact`) and the sticky top nav links to them; the nav highlights the section in view with an IntersectionObserver.
  - Work cards are `<article class="card" data-cat="vr|web|jam">` inside `#grid`. The filter buttons in `.filters` carry `data-filter` values that must match a `data-cat`; the script appends the per-category counts and hides non-matching cards with the `hidden` attribute. Add `class="featured"` to make a card span two columns.
  - A card's `<button class="card-media" data-full="…" data-caption="…">` opens the native `<dialog id="lightbox">` with that image; add `data-video="…mp4"` to play a video instead (`data-full` is then the poster). A `.card-media.static` div is a decorative tile that does not open the lightbox.
  - Images live in `assets/img/portfolio/` as WebP (~1400 px wide, converted with `cwebp`); the original key art for the Kodansha, Tokyo Dome and XR Center titles was downloaded from their official sites. `super-tennis-deluxo.svg` is a hand-drawn placeholder because the live game has no public key art. `portafolio4.mp4` (9 MB, moov atom at the end, so it only plays after a full download) is the Game Boy clip shown in the lightbox.
  - Counters use `data-count`, reveal-on-scroll uses `.reveal` → `.in`, and the hero word rotator is `#rotator`. Everything honors `prefers-reduced-motion`.
  - There is no contact form; contact is a `mailto:` link.
- **Unity WebGL builds**: `ARTest/` (Unity 2020+ with the Zappar WebAR template), `ARTEST2/` (older UnityLoader build bridged to A-Frame + AR.js marker tracking; a Three.js variant is in `ThreeVersion/`) and `roomy/` (newer Unity build). These are build outputs exported from Unity, so rebuild and replace them instead of hand-editing the minified loader or framework files. Each `index.html` is the Unity template page and can be edited.
  - `roomy/` ships Brotli-compressed files (`*.br`). GitHub Pages does not send `Content-Encoding: br`, so this build only loads if it was exported with Unity's "Decompression Fallback" turned on, or rebuilt with compression disabled. Check this first if roomy fails to load.
- **`PrivacyPolicy/index.html`**: the privacy policy for the PaniniWC2026 mobile app, which carries ads. App store listings link to this URL.
- **`app-ads.txt`**: the AdMob publisher declaration. It has to stay at the root with exactly this name.

## Conventions

Commit messages in history use a `dev- <description>` prefix.
