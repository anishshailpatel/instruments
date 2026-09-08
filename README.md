# Instruments

Three standalone interactive exhibits, one per note on anishpatel.co. They are embedded by iframe and are the only files that ever go to the public origin.

| File | Note it belongs to |
|---|---|
| `five-shapes.html` | Five shapes |
| `what-a-slip-costs.html` | What a slip costs |
| `margin-or-growth.html` | Margin or growth |

## Where these live

**Not in the vault repo.** `anishshailpatel/notes` is private, tracks employer analysis and dictation transcripts, and must never be a GitHub Pages source. That is why this folder sits outside it.

These three files go to a separate public repo containing nothing else, served over https, per the verdict in `AP_Notes/_site-planning/06-platform-switch-case.md`. Each file is self-contained: no build step, no dependencies, no third-party assets beyond the Google Fonts stylesheet.

## What is already built in

- `noindex, nofollow`, so they never appear in search results on their own.
- A top-level redirect: opened directly rather than in a frame, each one sends the visitor to its note on anishpatel.co. Change those URLs in the file if the note paths change.
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

`instruments.anishpatel.co` is a placeholder. Replace it in all three notes once the origin exists.

## Still outstanding before a real publish

A static SVG fallback inside each note, so a CSP change degrades to a picture rather than a hole. The plumbing proof on iOS Safari at phone width, through a live theme flip, and in Obsidian desktop and mobile reading view.
