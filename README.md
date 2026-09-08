# Instruments

Three standalone interactive exhibits, one per note on anishpatel.co. They are embedded by iframe and are the only files that ever go to the public origin.

| File | Note it belongs to |
|---|---|
| `five-shapes.html` | Five shapes |
| `what-a-slip-costs.html` | What a slip costs |
| `margin-or-growth.html` | Margin or growth |

## Getting them live

Nothing serves these yet, which is why the notes currently show a DNS error. This folder is already a git repo with one commit, so publishing is two steps.

**Option A, no DNS.** Create a public repo called `instruments` under `anishshailpatel`, push, and turn on Pages from the `main` branch root.

```bash
cd "C:/Users/anish/Dropbox/Code/instruments"
git remote add origin https://github.com/anishshailpatel/instruments.git
git branch -M main
git push -u origin main
```

The files then serve at `https://anishshailpatel.github.io/instruments/five-shapes.html`. This is the fastest route and needs no Cloudflare work.

**Option B, custom subdomain.** As above, then add a `CNAME` file containing `instruments.anishpatel.co`, and point that name at `anishshailpatel.github.io` in Cloudflare with the proxy off.

Either way, replace `https://instruments.anishpatel.co/` in the three notes with whichever base URL you chose, and change the `location.replace` target at the top of each file if the note paths differ.

## Why not the vault repo

`anishshailpatel/notes` is private, tracks employer analysis and dictation transcripts, and must never be a GitHub Pages source. That is why this folder sits outside it, per the verdict in `AP_Notes/_site-planning/06-platform-switch-case.md`.

## What is already built in

- `noindex, nofollow`, so they never appear in search results on their own.
- A top-level redirect: opened directly over https rather than in a frame, each one sends the visitor to its note on anishpatel.co. It deliberately does not fire on `file://` or `localhost`, so you can open these locally to check them.
- Light and dark palettes from `prefers-color-scheme`, so they follow the reader's theme with Publish set to adapt to system.
- A `postMessage` height report, which nothing currently listens to. The notes use fixed heights instead, per the version-one embed contract. It is there if a `publish.js` height shim is ever wanted.

## The embed, as used in the notes

```html
<iframe src="https://instruments.anishpatel.co/five-shapes.html"
        title="Five shapes a monthly number takes"
        width="100%" height="1200"
        style="width:100%;border:0;display:block;margin:1.2em 0;"
        loading="lazy" sandbox="allow-scripts allow-same-origin"></iframe>
```

Heights are set from the tallest measured render, a 375px phone, with headroom: measured 1149, 1132 and 1357, set to 1200, 1180 and 1400. Measure again after any edit that adds a row or a control.

## Checked

At 375px and at 760px, in light and in dark, embedded in a host page by iframe. No console errors, no inner scrollbar, no sideways scroll, every control reachable. Both `html` and `body` are transparent in all three, so each widget composites onto the host's own background and follows the reader's theme.

The live site has now shown that Obsidian Publish's sanitiser keeps the iframe and renders it, which was the open question. Only the origin was missing.

## Still outstanding before a real publish

A static SVG fallback inside each note, so a CSP change degrades to a picture rather than a hole. The plumbing proof on iOS Safari at phone width, through a live theme flip, and in Obsidian desktop and mobile reading view.
