# spicetify-grid-coordinator

Shared tracklist grid layout for Spicetify extensions that add their own column to Spotify's tracklist
(for example Star Ratings, DJ Info, DJ Prep).

Spotify drives each row's `grid-template-columns` from its own CSS variables. If every extension rewrites that
property, the columns overlap or overwrite each other. This module is the single place that fits the grid:
it widens the native last column just enough, inserts a named track for every extension column, and
re-checks the result against the computed layout.

## Use

Copy or vendor `grid-coordinator.ts` and import it. It is meant to be **bundled into each extension**, not
installed separately, so every extension keeps working on its own.

```ts
import { GridCoordinator } from "./grid-coordinator";
GridCoordinator.scheduleFit(true);
```

Extension cells opt in with a class (`.djInfoSeparateCol`, `.starRatingsSeparateCol`, `.djPrepSeparateCol`) and may set
`data-ext-col-min="<px>"` to declare their own minimum width.

## Coexistence

The API is exposed as `window.SpicetifyGridCoordinator` with a `version` number. Only the newest copy on the
page runs; an older one is cleaned up when a newer one loads. Bump `version` when behaviour changes.

## Side-by-side sections

On pages where the tracklist shares a row with other sections (artist page: Popular / You Liked / Artist pick), the coordinator
only acts if the rows would overflow into their neighbours. It then gives the tracklist section the width it needs and lets the
neighbours wrap below when there is no room. It uses the page structure, not Spotify's generated class names.

## Add a new column

Add an entry to `EXTRA_COL_DEFS` in `grid-coordinator.ts` (key, selector, default width).

## Versions

- 4: tracklists next to other sections (artist page) get the width their rows need; neighbours wrap below when there is no room.
- 3: cells can declare `data-ext-col-min`.
- 2: separate-column support.
