# Bulk URL Shortener

A static web app you can host on **GitHub Pages** for bulk URL shortening.

Paste ~1000 URLs → get back a list of short links like:

```
https://<your-username>.github.io/new/?s=Ab3xY9
```

When someone visits that short link, the app itself reads the `?s=<CODE>` parameter and redirects to the original long URL.

## Current deployment

- Site: `https://mimohivi4.github.io/new/`
- Short link format: `https://mimohivi4.github.io/new/?s=<CODE>`

## Technique (new)

This version uses a **query-parameter based redirect** handled by `index.html` itself:

- Short link format: `https://<base>/?s=<CODE>`
- On page load, the app checks for `?s=<CODE>`.
- If present, it looks up the code in `shortcodes.json` and redirects to the full long URL.

This replaces the previous path-based (`/<CODE>`) approach that relied on GitHub Pages' `404.html` fallback (which kept failing).

## Notes

- The app detects its own base path automatically (`/new/`, `/pro/`, `/`, etc.), so it works at any GitHub Pages sub-path without code changes.
- The redirect target is the **full long URL** stored in `shortcodes.json`. If an entry looks truncated (ending at `?xml=data:gsf`), re-run the shorten flow with the complete URLs pasted.

## Features

- 📥 Bulk paste & processing (new lines / commas / spaces as separators)
- 🏷️ Friendly titles via `URL | Custom Title` format
- 🔗 Auto-generated 6-char short codes, deduplication, duplicate stats
- 📋 Copy individual / all short URLs, copy as Markdown, download `shortcodes.json`
- 🔳 QR code generation per short link
- 🌙 Dark mode toggle

## Files

| File | Purpose |
|---|---|
| `index.html` | The bulk shortener app (paste URLs, get short links, copy/download) |
| `404.html` | Legacy redirect handler (fallback, optional) |
| `shortcodes.json` | Mapping of short code → original long URL (e.g. `{ "Ab3xY9": "https://example.com/very/long-url" }`) |

## Deploy to GitHub Pages

1. Create a new repository named `mimohivi4/new` (for a project site) or use any repo and enable GitHub Pages.
2. Place `index.html`, `404.html`, and `shortcodes.json` in the **root** of the repo.
3. Push to the `main` branch.
4. Go to **Settings → Pages** and set Source to **Deploy from a branch → main / (root)**.
5. Wait a minute and visit `https://mimohivi4.github.io/new/`.

## Usage

1. Open your new GitHub Pages site.
2. Paste up to ~1000 URLs into the textarea (one per line, or comma/space separated).
3. Click **⚡ Shorten URLs**.
4. Copy individual short links, copy all, or download `shortcodes.json`.
5. **Commit the downloaded `shortcodes.json`** to the root of your repo so redirects resolve for everyone.

## Short link format

```
https://<your-username>.github.io/new/?s=<CODE>
```

Codes are 6-character mixed-case alphanumeric (base58-ish alphabet, excludes ambiguous characters like `0/O`, `1/I/l`).
