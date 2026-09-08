# Instruments

Three standalone interactive exhibits, one per note on anishpatel.co. They are embedded by iframe and are the only files that ever go to the public origin.

| File | Note it belongs to |
|---|---|
| `five-shapes.html` | Five shapes |
| `what-a-slip-costs.html` | What a slip costs |
| `margin-or-growth.html` | Margin or growth |

## Where they serve from

GitHub Pages, from `main` at the repository root: `https://anishshailpatel.github.io/instruments/five-shapes.html`. That is the base URL the three notes point at.

Neither `raw.githubusercontent.com` nor jsDelivr works as an alternative: both serve `.html` as `text/plain`, so an iframe shows source rather than a page.

**Later, if a custom subdomain is wanted.** Add a `CNAME` file containing `instruments.anishpatel.co`, point that name at `anishshailpatel.github.io` in Cloudflare with the proxy off, then swap the base URL in the three notes and the `location.replace` target at the top of each file.

## Why not the vault repo

`anishshailpatel/notes` is private, tracks employer analysis and dictation transcripts, and must never be a GitHub Pages source. That is why this folder sits outside it, per the verdict in `AP_Notes/_site-planning/06-platform-switch-case.md`.

## What is already built in

- `noindex, nofollow`, so they never appear in search results on their own.
- A top-level redirect: opened directly over https rather than in a frame, each one sends the visitor to its note on anishpatel.co. It deliberately does not fire on `file://` or `localhost`, so you can open these locally to check them.
- Inter at 16px, matching the host site, so an exhibit reads as part of the page rather than an imported object. Only two families load: Inter and a mono for the working panel.
- Light and dark palettes, taken from the host page where it says, and from `prefers-color-scheme` where it does not.
- A conditional ground. Sitting on the host's own background is what makes an exhibit read as part of the page, and it is only safe once the host has named its theme. Until then the frame paints its own ground and stands as a card. Without that, a frame following a dark machine on a light page puts dark panels and near-white figures on white, which is the one failure mode that makes an exhibit unreadable rather than merely unblended.
- A `postMessage` pair with the host, handled by `AP_Notes/publish.js` in the vault. The frame announces itself on load and accepts `{publishTheme}` back; it reports its own height as `{instrument, height}` and the host sets the frame to match.

## The embed, as used in the notes

```html
<iframe src="https://instruments.anishpatel.co/five-shapes.html"
        title="Five shapes a monthly number takes"
        width="100%" height="1260"
        style="width:100%;border:0;display:block;margin:1.2em 0;"
        loading="lazy" sandbox="allow-scripts allow-same-origin"></iframe>
```

Once `publish.js` is live the host sets the height from what the frame reports, so the attribute is only a floor for the first paint and for Obsidian's own reading view, where `publish.js` does not run. Err tall: a frame short by a row clips the exhibit, where one long by a row leaves a gap.

The floors are the tallest measured render, a 375px phone, with headroom.

| File | 375px | 700px | Attribute |
|---|---|---|---|
| `five-shapes.html` | 1113 | 1025 | 1140 |
| `what-a-slip-costs.html` | 1076 | 956 | 1120 |
| `margin-or-growth.html` | 1514 | 1256 | 1560 |

## What each one models

**Five shapes** plots five rows of a stylised pack against limits drawn from their own first eight months. The rows are new bookings, revenue, gross margin, overheads and debtor days, chosen because none is arithmetically derived from another, so each can carry one shape on its own. An earlier version used cost of sales and EBITDA, and a reader who added the P&L up found it did not tie. Favourable direction is per row: a cost or a debtor day falling is green.

**What a slip costs** is a five-year undiscounted cash line for a build whose benefit only starts at the first buying window at or after it is ready, with a 40 / 75 / 100 adoption ramp.

**Margin or growth** compares a cost programme with an expansion on a Gordon-growth perpetuity, and carries a third control for how long the faster growth lasts. For ever is the perpetuity; the finite settings run the faster growth and the new-market return for that many years, then fall back to today's growth at the core's return and discount the rest. On the defaults the expansion is worth £250m for ever and £144m if 8% lasts five years, against a £162m cost programme, and it takes about sixteen years of 8% to draw level.

## Sized for the note column, not the phone

Cards, tables, controls and readouts take the full width of the column they are given. Only the drawing itself is capped, at 560px, and the cap is not arbitrary: the charts are laid out on a 345-unit grid with 10.5-unit type, so 560px renders that type at 16px, which is exactly the body size on the host site. A wider drawing would make the chart labels larger than the prose around them.

## Checked

At 375px and at 700px, in light and in dark, embedded in a host page by iframe, with the theme flipped live from the host after load. No console errors, no inner scrollbar, no sideways scroll, every control reachable. Both `html` and `body` are transparent, so each exhibit composites onto the host's own background.

The live site has shown that Obsidian Publish's sanitiser keeps the iframe and renders it, which was the open question.

## Still outstanding before a real publish

A static SVG fallback inside each note, so a CSP change degrades to a picture rather than a hole. The plumbing proof on iOS Safari at phone width and in Obsidian desktop and mobile reading view. `publish.js` uploaded from the vault root, without which the theme falls back to the reader's operating system and the heights stay at their floors.
