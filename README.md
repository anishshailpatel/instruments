# Instruments

Three standalone interactive exhibits, one per note on anishpatel.co. They are embedded by iframe and are the only files that ever go to the public origin.

| File | Note it belongs to |
|---|---|
| `five-shapes.html` | Five shapes |
| `what-a-slip-costs.html` | What a slip costs |
| `margin-or-growth.html` | Margin or growth |

## Getting them live

The repo exists and `main` is pushed. One step remains: **turn on GitHub Pages**, at Settings, Pages, source Deploy from a branch, branch `main`, folder `/ (root)`. It takes about a minute to go live.

The files then serve at `https://anishshailpatel.github.io/instruments/five-shapes.html`, which is the base URL the three notes already point at. Nothing else needs changing.

Neither `raw.githubusercontent.com` nor jsDelivr works as a stopgap: both serve `.html` as `text/plain`, so an iframe shows source rather than a page.

**Later, if a custom subdomain is wanted.** Add a `CNAME` file containing `instruments.anishpatel.co`, point that name at `anishshailpatel.github.io` in Cloudflare with the proxy off, then swap the base URL in the three notes and the `location.replace` target at the top of each file.

## Why not the vault repo

`anishshailpatel/notes` is private, tracks employer analysis and dictation transcripts, and must never be a GitHub Pages source. That is why this folder sits outside it, per the verdict in `AP_Notes/_site-planning/06-platform-switch-case.md`.

## What is already built in

- `noindex, nofollow`, so they never appear in search results on their own.
- A top-level redirect: opened directly over https rather than in a frame, each one sends the visitor to its note on anishpatel.co. It deliberately does not fire on `file://` or `localhost`, so you can open these locally to check them.
- Inter at 16px, matching the host site, so an exhibit reads as part of the page rather than an imported object. Only two families load: Inter and a mono for the working panel.
- Light and dark palettes from `prefers-color-scheme`, so they follow the reader's theme with Publish set to adapt to system.
- A `postMessage` height report, which nothing currently listens to. The notes use fixed heights instead, per the version-one embed contract. It is there if a `publish.js` height shim is ever wanted.

## The embed, as used in the notes

```html
<iframe src="https://instruments.anishpatel.co/five-shapes.html"
        title="Five shapes a monthly number takes"
        width="100%" height="1260"
        style="width:100%;border:0;display:block;margin:1.2em 0;"
        loading="lazy" sandbox="allow-scripts allow-same-origin"></iframe>
```

Heights are set from the tallest measured render, a 375px phone, with headroom: measured 1199, 1151 and 1355, set to 1260, 1215 and 1420. Measure again after any edit that adds a row or a control.

## Checked

At 375px and at 760px, in light and in dark, embedded in a host page by iframe. No console errors, no inner scrollbar, no sideways scroll, every control reachable. Both `html` and `body` are transparent in all three, so each widget composites onto the host's own background and follows the reader's theme.

The live site has now shown that Obsidian Publish's sanitiser keeps the iframe and renders it, which was the open question. Only the origin was missing.

## Still outstanding before a real publish

A static SVG fallback inside each note, so a CSP change degrades to a picture rather than a hole. The plumbing proof on iOS Safari at phone width, through a live theme flip, and in Obsidian desktop and mobile reading view.
