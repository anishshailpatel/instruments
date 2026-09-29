# Instruments

Five thinking tools. Each one works two ways: embedded by iframe inside the note on anishpatel.co that explains it, and on its own, opened directly, where it carries its own name, the question it answers and a link to that note (AP's call, 2026-09-29: the tools should make sense in isolation, so they can be reused or gathered into a library of thinking tools later). They are the only files that ever go to the public origin.

| File | Tool | Note that explains it |
|---|---|---|
| `five-shapes.html` | Signal or noise | Reading the numbers |
| `margin-or-growth.html` | Margin or growth | Facing into M&A |
| `the-cycle.html` | What a slow cycle costs | Waiting |
| `what-a-slip-costs.html` | What a slip costs | Waiting |
| `road-by-road.html` | Spread thin | Building density |

Filenames are stable and no longer match the tools' names, because the embed URLs had to keep working through renames. The link in each file's header is the thing that tracks the note.

**These files are the source.** The earlier master (`the-instruments.html`) and its `extract.py` lived in a session's temporary folder and no longer exist, so edit the five files directly. The shared styles and helpers are duplicated in each; change them in all five. Before any push, dump every reader-facing string with the vault's `_tools/widget-copy.py <file>`, lint it with `preflight.ps1`, and read the header copy too, which the dump does not see.

## Where they serve from

GitHub Pages, from `main` at the repository root: `https://anishshailpatel.github.io/instruments/five-shapes.html`. That is the base URL the three notes point at.

Neither `raw.githubusercontent.com` nor jsDelivr works as an alternative: both serve `.html` as `text/plain`, so an iframe shows source rather than a page.

**Later, if a custom subdomain is wanted.** Add a `CNAME` file containing `instruments.anishpatel.co`, point that name at `anishshailpatel.github.io` in Cloudflare with the proxy off, then swap the base URL in the notes.

## Why not the vault repo

`anishshailpatel/notes` is private, tracks employer analysis and dictation transcripts, and must never be a GitHub Pages source. That is why this folder sits outside it, per the verdict in `AP_Notes/_site-planning/06-platform-switch-case.md`.

## What is already built in

- `noindex, nofollow`, so they never appear in search results on their own.
- A standalone mode. Opened directly rather than in a frame, or framed with `?solo=1` (for a future library page), the page adds the class `solo` to `html`, which shows a header: "Thinking tool", the tool's name, a sentence on the question it answers, and "The thinking behind it is in" its note. Embedded in a note, the header stays hidden and the tool starts at its card. This replaced a redirect to the note on 2026-09-29.
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

The floors are the tallest measured embedded render, a 351px column on a phone, with headroom (remeasured 2026-09-29 after the new controls).

| File | 351px | Attribute |
|---|---|---|
| `five-shapes.html` | 1201 | 1240 |
| `what-a-slip-costs.html` | 1177 | 1220 |
| `margin-or-growth.html` | 1615 | 1660 |
| `road-by-road.html` | 983 | 1020 |
| `the-cycle.html` | 984 | 1040 |

## What each one models

**Five shapes** plots five rows of a stylised pack against limits drawn from their own first eight months. The rows are new bookings, revenue, gross margin, overheads and debtor days, chosen because none is arithmetically derived from another, so each can carry one shape on its own. The tabs name the row rather than the shape, in the order the rows sit in the pack, and the note's prose and the readout supply the shape word; gross margin is the default because it is the row a variance table waves through. An earlier version used cost of sales and EBITDA, and a reader who added the P&L up found it did not tie. Favourable direction is per row: a cost or a debtor day falling is green.

**What a slip costs** is a five-year undiscounted cash line for a build whose benefit only starts at the first buying window at or after it is ready, with a 40 / 75 / 100 adoption ramp.

**Margin or growth** compares a cost programme with an expansion on a Gordon-growth perpetuity. Its controls are the four numbers the note says the case rests on: the growth rate, the return the new capital earns, how long the faster growth lasts, and the hurdle the new market has to clear. The last discounts only the cash the expansion adds, over the business as it is, at the higher rate, so the core keeps its own value; it starts level with the core's hurdle, which is the kind case, and at 12% on the defaults the expansion is worth £164m against the cost programme's £162m. For ever is the perpetuity; the finite settings run the faster growth and the new-market return for that many years, then fall back to today's growth at the core's return and discount the rest. On the defaults the expansion is worth £250m for ever and £144m if 8% lasts five years, against a £162m cost programme, and it takes about sixteen years of 8% to draw level.

**Spread thin** (`road-by-road.html`) keeps your total customers fixed and spreads them across one to ten equal patches against a rival who keeps twenty in one. A switch sets what the customers in a patch share. On a route, the default, the cost of reaching each one follows the square root, as below. On a fixed cost it falls one for one, so your cost per customer against the rival's is simply rival's customers ÷ yours per patch: 0.2 at one patch, level at five, twice at ten. The cost of reaching a customer in a patch scales with the square root of area over customers (the travelling-salesman result), so your cost per customer against the rival's is the square root of (rival's customers ÷ your customers per patch): 0.45 at one patch, 1.0 at five, 1.41 at ten. The total never enters except through how thinly it is spread. The wording is deliberately generic (customers, patches, cost to reach) rather than vans and roads, so the lesson travels; the filename keeps its old name for URL stability.

**The cycle** charges one cost of a slow sales process: a live deal has a fixed chance of dying each month it waits (15% by default), so survival to decision is (1 − d)^N. Won a month = leads × survival × win rate; deals in flight = leads × (1 + (1 − d) + …) over N months; the line is cumulative revenue from month N + 1 over two years against a fixed one-month reference. Defaults: 100 leads, 25% win rate, £20k a deal. At three months that is 39% of deals lost, about 15 won a month against 21 (the readout rounds deal counts to whole numbers), £6.4m against £9.8m over two years, and 257 in flight against 100.

## Sized for the note column, not the phone

Cards, tables, controls and readouts take the full width of the column they are given. Only the drawing itself is capped, at 560px, and the cap is not arbitrary: the charts are laid out on a 345-unit grid with 10.5-unit type, so 560px renders that type at 16px, which is exactly the body size on the host site. A wider drawing would make the chart labels larger than the prose around them.

## Checked

At 375px and at 700px, in light and in dark, embedded in a host page by iframe, with the theme flipped live from the host after load. No console errors, no inner scrollbar, no sideways scroll, every control reachable. Both `html` and `body` are transparent, so each exhibit composites onto the host's own background.

The live site has shown that Obsidian Publish's sanitiser keeps the iframe and renders it, which was the open question.

## Still outstanding before a real publish

A static SVG fallback inside each note, so a CSP change degrades to a picture rather than a hole. The plumbing proof on iOS Safari at phone width and in Obsidian desktop and mobile reading view. `publish.js` uploaded from the vault root, without which the theme falls back to the reader's operating system and the heights stay at their floors.
