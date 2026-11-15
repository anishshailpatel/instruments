# Instruments

Eight thinking tools. Each one works two ways: embedded by iframe inside the note on anishpatel.co that explains it, and on its own, opened directly, where it carries its own name, the question it answers and a link to that note (AP's call, 2026-09-29: the tools should make sense in isolation, so they can be reused or gathered into a library of thinking tools later). They are the only files that ever go to the public origin.

| File | Tool | Note that explains it |
|---|---|---|
| `five-shapes.html` | Signal or noise | none (removed from Reading the numbers, 2026-10-25) |
| `margin-or-growth.html` | Margin or growth | none (removed from Facing into M&A, 2026-10-18; fix its 10% bug before any reuse) |
| `the-cycle.html` | What a slow cycle costs | Waiting |
| `what-a-slip-costs.html` | What a slip costs | Waiting |
| `road-by-road.html` | Spread thin | Building density |
| `checking-in.html` | Checks that find nothing | Checking in |
| `ten-projects.html` | Ten two-month projects | What estimates mean (built 2026-11-14, embedded 2026-11-14) |
| `one-person.html` | The pile in front of one person | Human bottlenecks (built 2026-11-15; goes live when the note embeds it) |

Filenames are stable and no longer match the tools' names, because the embed URLs had to keep working through renames. The link in each file's header is the thing that tracks the note.

**These files are the source.** The earlier master (`the-instruments.html`) and its `extract.py` lived in a session's temporary folder and no longer exist, so edit the five files directly. The shared styles and helpers are duplicated in each; change them in all five. Before any push, dump every reader-facing string with the vault's `_tools/copy-dump.py <file>` (it reads the header and meta description as well as the script copy), lint the dump with `preflight.ps1`, then read it as prose.

## Where they serve from

GitHub Pages, from `main` at the repository root: `https://anishshailpatel.github.io/instruments/five-shapes.html`. That is the base URL the four notes point at.

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
<iframe src="https://anishshailpatel.github.io/instruments/five-shapes.html"
        title="The same five rows plotted: two of noise, two signals and a contaminated baseline"
        width="100%" height="1240"
        style="width:100%;border:0;display:block;margin:1.2em 0;"
        loading="lazy" sandbox="allow-scripts allow-same-origin"></iframe>
```

Once `publish.js` is live the host sets the height from what the frame reports, so the attribute is only a floor for the first paint and for Obsidian's own reading view, where `publish.js` does not run. Err tall: a frame short by a row clips the exhibit, where one long by a row leaves a gap.

The floors are the tallest measured embedded render, a 351px column on a phone, with headroom. Remeasure any file whose rows change before pushing, and update this table and the note's `height` together.

Remeasured 2026-11-02 for the four embedded tools, after the cards, working panels and speed control came out (method as below; the two unembedded tools keep their October figures). Before that, remeasured 2026-10-10 twice: once after the spine, ticker and run clock went in, and again after the readouts were cut back and the guides and working-panel essays came out. The figures below are the second pass, in a 351px iframe on the build machine, sweeping every tab, band, segment and both ends of every slider. Those renders come out a few per cent under a real phone's, which is why the attribute carries about 12% over the figure beside it.

| File | 351px | Attribute | Was |
|---|---|---|---|
| `five-shapes.html` | 1124 | 1280 | 1240 |
| `what-a-slip-costs.html` | 1028 | 1150 | 1220 |
| `margin-or-growth.html` | 1575 | 1780 | 1660 |
| `road-by-road.html` | 632 | 710 | 980 |
| `the-cycle.html` | 1072 | 1200 | 1200 |
| `checking-in.html` | 802 | 900 | 1060 |
| `ten-projects.html` | 595 | 670 | new |
| `one-person.html` | 726 | 815 | new |

The net of the 2026-10-10 changes was close to nothing: the prose that came out paid for the spine that went in. The 2026-11-02 pass took a further 130 to 230px out of three of the four.

## The spine, the ticker and the clock (2026-10-10)

Taken from [john.fun/elevators](https://john.fun/elevators), which AP liked: it puts its optimiser's scoring formula on the page as named boxes rather than hiding it, and it lets the reader run the simulation and watch the statistics accumulate.

**The spine** is in all five. It is the one line of arithmetic the tool turns on, sitting between the controls and the readout: each term named in two or three words with its live value beside it, operators between, and the answer in the accent box. `spine(id, label, terms)`, where a string in `terms` is an operator and a pair is `[name, value]`, with a third member `"out"` marking the answer. It carries labels and numbers only. An earlier draft gave it a sentence of explanation underneath; that was cut on the day under AP's rule that text inside the tools is a cost to justify, and the working panel already carried the same explanation where it was worth keeping.

The terms have to multiply out to the answer at the precision shown. `five-shapes` prints its spread one decimal finer than the series itself for exactly this reason: at the series' own precision, 2.66 × 0.2 lands a rounding short of the band it draws.

**The ticker** and **the clock** are in `the-cycle.html` only, because it is the one tool whose subject is time passing. The clock runs at a fixed four months a second, two years in six (a speed control came out on 2026-11-02 as performance rather than teaching). It advances a horizon counter `T` from a standing start to twenty-four months, redrawing the cumulative lines clipped to it, while the ticker counts revenue won, deals in flight and deals lost while waiting, for the chosen cycle against the one-month reference. The figures are expected values rather than a draw, so they tie to the working panel exactly.

**End labels** in `the-cycle.html` and `what-a-slip-costs.html` are placed by `placeLabels()`: each starts level with the end of its own line, the stack keeps the lines' top-to-bottom order, and a label nudged more than a few units gets a faint leader back to its line. Until 2026-11-02 the accent label was placed first and the others dodged it, which on the Monthly tab put "+£1.9m on plan" beside the twelve-months-late line. Keep the two copies of the function identical.

Three things it deliberately does not do. It never autoplays, because these sit inside something somebody is reading. It starts at the finish, so a reader who never touches the button sees exactly the picture the tool showed before any of this existed. And during a run it updates only the drawing, the ticker and the clock, leaving the readout alone: the readout is `aria-live="polite"`, and rewriting it sixty times a second would spam a screen reader with a sentence that has not changed.

## What each one models

**Five shapes** plots five rows of a stylised pack against limits drawn from their own first eight months. The rows are new bookings, revenue, gross margin, overheads and debtor days, chosen because none is arithmetically derived from another, so each can carry one shape on its own. The tabs name the row rather than the shape, in the order the rows sit in the pack, and the note's prose and the readout supply the shape word; gross margin is the default because it is the row a variance table waves through. An earlier version used cost of sales and EBITDA, and a reader who added the P&L up found it did not tie. Favourable direction is per row: a cost or a debtor day falling is green.

**What a slip costs** is a five-year undiscounted cash line for a build whose benefit only starts at the first buying window at or after it is ready, with a 40 / 75 / 100 adoption ramp. `poundsK` rounds to whole £k before it rounds to the displayed £10k, because two of the case's five-year figures (£735k yearly, −£165k every three years) sit exactly on a boundary, and floating-point noise was rounding the same figure up in one place and down in another. Its working panel came out on 2026-11-02: it repeated the note's own callout.

**Margin or growth** compares a cost programme with an expansion on a Gordon-growth perpetuity. Its controls are the four numbers the note says the case rests on: the growth rate, the return the new capital earns, how long the faster growth lasts, and the hurdle the new market has to clear. The last discounts only the cash the expansion adds, over the business as it is, at the higher rate, so the core keeps its own value; it starts level with the core's hurdle, which is the kind case, and at 12% on the defaults the expansion is worth £164m against the cost programme's £162m. For ever is the perpetuity; the finite settings run the faster growth and the new-market return for that many years, then fall back to today's growth at the core's return and discount the rest. On the defaults the expansion is worth £250m for ever and £144m if 8% lasts five years, against a £162m cost programme, and it takes about sixteen years of 8% to draw level.

**Spread thin** (`road-by-road.html`) keeps your total customers fixed and spreads them across one to ten equal areas against a rival who keeps twenty in one. A switch sets what the customers in an area share. On a route, the default, it measures ground covered per customer, which scales with the square root of area over customers (the travelling-salesman result), so your ground per customer against the rival's is the square root of (rival's customers ÷ your customers per area): 55% less at one area, level at five, 41% more at ten. It bounds the driving and nothing else; time at the door is the same for everyone. On a fixed cost the cost per customer falls one for one, so yours against the rival's is simply rival's customers ÷ yours per area: 80% less at one area, level at five, twice at ten. The total never enters except through how thinly it is spread. The wording is deliberately generic (customers, areas, ground or cost) rather than vans and roads, so the lesson travels, and it uses the note's word, areas (it said patches until 2026-11-02); the filename keeps its old name for URL stability.

The spine shows its terms exactly, as a division (√(20 ÷ (100 ÷ 6))), and its answer as the same rounded percentage the bar and the readout carry, so the three always agree; it used to print a two-decimal ratio from an unrounded figure beside a rounded one, and at six or eight areas the shown terms didn't work out to the answer. The card ("Market leader… expand into new patches" in green) came out on 2026-11-02, because nothing marked it as the pack talking and it read as the tool recommending the foothold move the note warns against; so did the working panel, which sat directly above the note's own working callout, and the footer under the drawing.

**Checks that find nothing** (`checking-in.html`, 2026-10-10) is built on the note's core insight rather than on its arithmetic. A first version sorted the reader's reports into the ones they still check on and the ones they trust, and priced the diary; AP rejected the frame twice, and he was right both times. It made the tool grade people, which the note never does, and it answered a question the note does not ask. The note's insight is that polling and interrupts are two mechanisms for the same job with different cost structures, and that polling wastes most of its effort when little is happening.

So the reader's control is how often something worth raising comes up, for each person, per month, which is a property of the work rather than of the person. A scheduled check is charged whether or not anything was there. At the default of once a month, 77% of the checks find nothing; at four a month, only 8% do. The slider stops at four, because past one a week every check finds something and nothing on the page moves.

There is deliberately no "if raised instead" figure. Until 2026-11-02 the ticker showed 4.6h against 11.9h for the same coverage raised rather than scheduled, but that total assumes every one-to-one is cancelled, and the note says the opposite: keep them, and set the cadence per person. The note puts no number on the rate, only that most weeks, for most people, nothing has changed, so the default is a round one a month. The scheduled total is rounded once from the true figure (11.8h on the defaults), so it doesn't move when the rate does: how often something comes up changes what the checks find, never what they cost.

The exhibit is the week itself, five day-rows of half-hour slots, on AP's steer towards squares and a pseudo-calendar over another chart. Strong cells are one-to-ones that found something, pale cells are ones that found nothing and are the point of the picture, grey is the reviews and group meetings, and the blanks are what is left; a three-swatch key under the grid says so. Where the cells fall is drawn for legibility and the working says so; the count is the reader's. The card that repeated the ticker came out on 2026-11-02.

**The cycle** charges one cost of a slow sales process: a live deal has a fixed chance of dying each month it waits (15% by default), so survival to decision is (1 − d)^N. Won a month = leads × survival × win rate; deals in flight = leads × (1 + (1 − d) + …) over N months; the line is cumulative revenue from month N + 1 over two years against a fixed one-month reference. Defaults: 100 leads, 25% win rate, £20k a deal. At three months that is 39% of deals lost, about 15 won a month against 21 (the readout rounds deal counts to whole numbers), £6.4m against £9.8m over two years, and 257 in flight against 100.

**Ten two-month projects** (`ten-projects.html`, 2026-11-14) is the first of the tools that is a simulation, not a calculator: the reader presses a button and a plan of projects plays out. It exists because the note's curves show the shape of the spread but not what it is like to plan inside it, which is the note's reason the board gets the mean: you can't know which project will run, only that across enough of them the total lands near 1.8 times the estimates. The test proposed for a simulation, which AP said to build on (2026-11-14): the claim is counter-intuitive, it turns on chance or delay, and the reader's own click produces the result.

Each project's length is drawn from a lognormal whose most likely value is the two-month estimate and whose mean is 1.8 times it, the same curve `_tools/skew-curves.py` draws in the note (s² = ln 1.8 ÷ 1.5). Ten of them average 36.0 months against 20 planned, and come in at or under 20 about four times in a thousand. Grey is the estimate and the accent is the overrun. The ticker carries planned, this run, and the average across runs once there are two; the spine is estimates × 1.8 = the mean, which the running average converges on.

The rules it keeps, which any later simulation should keep too. It never autoplays. It opens on a fixed draw (seed 1821 + the number of projects, which at ten gives 1.5, 2.1, 10.2, 3.5, 1.7, 2.3, 4.2, 3.1, 4.0, 3.4, exactly 36.0), so the page looks the same on every arrival and the note can quote it; every run after that is a fresh draw. Lengths are rounded to a tenth as they are drawn, so the figures beside the bars add up to the total. With reduced motion a run lands at once. It carries labels and numbers only. Moving the slider (one to twenty projects) keeps the projects already on the page and adds or drops the last ones, and restarts the count of runs. The height above is the default; at twenty projects it is 726.

**The pile in front of one person** (`one-person.html`, 2026-11-15) is the second simulation, for *Human bottlenecks*, which had no visual. It is not the five-step factory line first proposed to AP. The note gives Goldratt's factory one paragraph and spends the rest on where a person differs from a machine, so the tool draws the person: their week as forty hour-squares, and under it the days of work waiting for them, a bar a week for a year. Three switches carry the note's three headings, in its order: clear their desk, let other work wait, add a second person. Pressing one plays the year; so does the button.

Nothing is drawn at random, because the note's claims here turn on delay, not chance. The figures are for illustration and the note quotes none of them: a 40-hour week with 16 hours on the work only they can do, 3 days of that work arriving a week, 10 days waiting at the start. Left alone the pile grows a day a week, to 62 days.

- **Clear their desk** moves 8 hours a week onto the work, which is exactly enough to stop the pile growing. Unless the second switch is on, those hours come back at an hour a fortnight (the note's "drifts back onto the bottleneck's desk"), and the year ends at 54 days: about two months bought.
- **Let other work wait** keeps the hours cleared, so the pile holds at 10. The counter beside it shows the price, 8 hours a week of other work held up. The switch is disabled until the desk is cleared, because there is nothing to hold before then.
- **Add a second person** costs the first person 8 hours a week of showing for six months, taken from their hours on the work, while the second person's share builds from nothing to 2 days a week. On its own the pile peaks at 35 days and is back to 9 by the year end, and for the first 24 weeks it stands above the do-nothing line, which is the only place the accent appears in the bars. After the other two switches it peaks at 16 and is gone by week 31. That gap is the note's reason for the order.

Once the pile is gone and the first person has hours to spare, the work that was left to wait comes back before any hour stands free, so the tool never shows other work held up beside an empty desk.

The week fills a row at a time, so a day of the work reads as a row. The ticker's first counter keeps a row to itself on a narrow column, because it grows a note mid-run ("35 at its worst") and nothing below it should move while a run plays. The spine follows the week on screen and its answer is worked from the terms as shown, to a tenth, so the row always adds up. Every figure is in hours or in days of work; nothing rates the person, and the labels say "the work only they can do", "everything else" and "showing the second person".

It keeps the simulation rules above. Its opening picture is the finished year with no switch on, and the height is the same whichever switches are on.

## Sized for the note column, not the phone

Cards, tables, controls and readouts take the full width of the column they are given. Only the drawing itself is capped, at 560px, and the cap is not arbitrary: the charts are laid out on a 345-unit grid with 10.5-unit type, so 560px renders that type at 16px, which is exactly the body size on the host site. A wider drawing would make the chart labels larger than the prose around them.

## Checked

At 375px and at 700px, in light and in dark, embedded in a host page by iframe, with the theme flipped live from the host after load. No console errors, no inner scrollbar, no sideways scroll, every control reachable. Both `html` and `body` are transparent, so each exhibit composites onto the host's own background.

The live site has shown that Obsidian Publish's sanitiser keeps the iframe and renders it, which was the open question.

## Still outstanding before a real publish

A static SVG fallback inside each note, so a CSP change degrades to a picture rather than a hole. The plumbing proof on iOS Safari at phone width and in Obsidian desktop and mobile reading view. `publish.js` uploaded from the vault root, without which the theme falls back to the reader's operating system and the heights stay at their floors.
