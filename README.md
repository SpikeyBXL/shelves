# Whisky shelves

A single-file shelf planner for a whisky collection kept on
[Whiskybase](https://www.whiskybase.com/). It lays the collection out across the
shelves you actually own, so you can see where you have room and where you don't.

Everything lives in `index.html` — no build step, no dependencies, no server.

## Running it

Open `index.html` in a browser. That's it.

To put it online, push this repository to GitHub and turn on Pages:

    Settings → Pages → Source: "Deploy from a branch" → main → / (root)

Rename `whisky-shelves.html` to `index.html` first, or Pages will serve a
directory listing instead of the app.

## Where the data lives

Your shelf state is kept in the browser's own storage, under the key
`whisky-shelf-plan-v4`. It never leaves the machine, which has two consequences:

- The layout does not follow you to your phone.
- Clearing site data wipes it. Use **Download CSV** now and then.

If you want it to sync, the honest options are a private Gist, a small
serverless key-value store, or committing the CSV back to this repository by
hand. None of them is free of friction.

## Updating the collection

Two ways in:

1. **Add a bottle** — for one-offs. Manual entries survive re-imports.
2. **Update from Whiskybase** — export your collection to CSV from Whiskybase
   (a Plus feature) and feed the file in. Bottles are matched on `CollectionID`.
   Notes and any region you set by hand are preserved; new bottles arrive on the
   right shelf; bottles you've sold or emptied disappear from the run.

A distillery the app doesn't recognise lands on the **Unsorted** shelf rather
than being guessed at. Assign it once and the choice sticks.

## Changing the shelving

The physical layout is defined near the top of the script:

    const SHELVES = [ {id:'L1', unit:'Left', name:'top', col:1, row:1}, … ];
    const ORDERS  = { rows: […], units: […] };
    const OFFSITE = 'Blends & bourbon';
    const UNIT3   = [ {id:'T1', …} ];

`SHELVES` is the grid of real shelves. `ORDERS` holds the two ways of walking
them. `OFFSITE` names the one region that is pulled out of the main run and
placed on `UNIT3`. Per-shelf capacities are editable in the interface and stored
with your data, not in the code.

Region assignments live in `MAP_DIST` and `MAP_BRAND`, keyed on the distillery
and brand strings Whiskybase uses. Add to them as the collection grows.
