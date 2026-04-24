# drifter

A flexion-biased keyboard layout for **heavily-staggered column-
staggered split boards** — Halcyon Elora, Kyria, and similar.
Drilled through seven iterations on a Halcyon Elora v2, built
around two biomechanical observations: **row direction is
asymmetric on aggressively-staggered col-stag hardware**, and
**hand-territory synchrony — whether both hands reach the same
rows during alternation — is a felt axis that generic scorers
don't name.**

drifter is not a general-purpose layout. It leans hard on
col-stag geometry: middle-column forward-thrust, pinky pull-back,
and a multi-key thumb cluster. On ANSI, flat ortho, or gentle
row-stag hardware the geometry isn't there to reward, and the
design wins disappear. **If your board isn't a heavily-staggered
col-stag split, drifter isn't the right layout for you** — try
Gallium, Canary, Graphite, or Colemak-DH instead, all of which
were designed against cost models that actually match your
hardware.

```
alpha grid:
  q v m j [    ] - = x z
  n r t s g    p h e a i
  b l d c w    k f u o y

left thumb  (k5, k6): enter, space
right thumb (k3..k6): ;  '  ,  .
```

The top row is low-traffic: rare letters (`q v m j x z`) plus
the ANSI-outer punctuation (`[ ] - =`) that doesn't fit
elsewhere. All frequent punctuation lives on the right thumb
cluster; **space and enter live on the left thumb**, so word
boundaries and line breaks don't interrupt the right-hand
typing rhythm.

## Why it exists

Most alt layouts (graphite, gallium, sturdy, ...) are designed
against cost models that treat the top and bottom rows as equally
costly. That assumption holds for flat ortho boards; on a
col-stag board with aggressive pinky pull-back and middle-finger
forward-thrust, the top row is meaningfully harder to reach than
the bottom. drifter is a layout designed around that asymmetry
instead of against it: **only rare letters on the top row**
(`q v m j x z`), both hands' common letters clustered in home +
bottom, and all frequent punctuation on the thumb cluster where
it doesn't interrupt alpha flow.

The same biomechanical observation was later codified as a
scoring model in [drift](https://github.com/kayplicated/keywiz/tree/master/drift),
a sibling project. drift isn't meant to replace oxeylyzer or
cmini — it's a different lens, tuned for the assumptions drifter
was drilled around. If the lens agrees with your hands, drifter
probably will too. If it doesn't, your layout probably lives
somewhere else on the design space, and that's fine.

## Design theses

**Hand-territory synchrony.** When prose streams by, alternation
bigrams land in matched row territory rather than splitting the
hands across vertical axes. Both hands' common letters sit in
home + bottom, so left-right-left and right-left-right patterns
stay flat instead of forcing the hands in opposite vertical
directions. It's an axis that doesn't appear in typical scorers;
drift names it `hand_territory`.

**Top row as a dead zone.** Common-letter top-row usage drops to
~5% in normal English prose. The flexion advantage of col-stag
geometry compounds here: the middle column's forward-thrust,
which makes reaching up expensive, makes curling down nearly
free — a finger that's already pre-extended forward barely moves
when it curls.

**The drift.** Both bottom rows work together as a single
rollable surface. The right-hand cluster `k f u o y` rolls
cleanly inward and outward through common bigrams — `of`/`fo`,
`ou`/`uo`, `ko`/`ok`, `you`/`uoy`, `oy`/`yo`, `ku`/`uk`. The
left-hand cluster `b l d c w` does the same on its side —
`dl`/`ld`, `bl`/`lb`, `wl`, the whole `d c w` triangle. And
words that span both halves (`would`, `words`, `world`,
`could`) *extend* the drift across the keyboard: left bottom
roll → cross-hand alternation → right bottom roll, with the
hands passing the motion between them rather than each side
working in isolation. That's where the name comes from — the
fingers drift along the same horizontal band instead of
bouncing across rows.

**Thumbs carry the most-used keys.** On col-stag boards with a
multi-key thumb cluster, the thumbs are the strongest and
fastest fingers on the hand — wasting them on a single `space`
key (the ANSI default) is a design loss. drifter uses both:

- **Left thumb: `space` and `enter`.** Word boundaries and line
  breaks don't interrupt the right-hand typing rhythm.
- **Right thumb: `; ' , .`** — all frequent punctuation. The
  Elora's thumb cluster makes thumb-reach cheap enough that
  even mid-word characters like `'` work well there —
  contractions (`don't`, `it's`, `they're`) flow through the
  thumb without interrupting alpha rhythm, and freeing `'` off
  the alpha grid shortens the common `i'` reach from pinky-to-
  index-outer-top to pinky-to-thumb. `; , .` don't appear
  mid-word in prose anyway, so their thumb placement is free.

Both the left-thumb space/enter and the right-thumb punctuation
are prescribed by `drifter.json`. On col-stag boards with fewer
thumb keys than the Elora's seven-per-hand (Kyria, Corne,
Ferris), you'll need to drop some bindings or map them to
combos; the alpha grid is what's essential, the thumb layout is
the preferred arrangement when the hardware allows it.

**`e` on the middle finger.** Piano-informed finger hierarchy
beats typing-community dogma. "Index is the strongest finger" is
a typing myth from the qwerty era; middle and ring are the power
fingers for sustained work. The most-frequent letter belongs on
the most-sustainable finger.

## What survived every iteration

- **`nrtsg / phaei` home row**, inherited from gallium. Every
  attempt to change it broke more than it fixed — the bigram
  structure of English pins these letters to those finger
  positions.
- **`sc` SFB on left-index.** Unavoidable given the home row;
  every relocation of `c` created worse SFBs somewhere else.
- **Left hand's common consonants on the bottom row**, rare
  letters on top. (Exact letter placement shifted across
  iterations — `v↔w` settled in the current form.)
- **Right hand's common letters on home + bottom, never top.**

## How it evolved

| Version | What changed                                 | What it taught                                                                                     |
|---------|----------------------------------------------|----------------------------------------------------------------------------------------------------|
| v1–v4   | Early row-flip experiments off gallium       | Flipping rows works; specific letter placements need tuning                                        |
| v5      | Stabilized the flipped structure             | `y` on pinky-bot is the wrong position; `sc` SFB is structural                                     |
| v6      | `a↔e` swap (e on middle finger)              | Middle is the power finger; `e` belongs there                                                      |
| v7      | `j`/`x`/`z` top-row rearrangement, `, ; .` → thumb | Empty alpha slots + thumb-bound punctuation unlocks clean trigram flow                             |
| v8      | Upside-down test (rows swapped)              | Structure survives inversion, but flexion is a real independent benefit — both axes carry the win |

## Using it

### Try it first

If you're considering drifter but haven't committed to the
col-stag hardware yet, [keywiz](https://github.com/kayplicated/keywiz)
ships with drifter as one of its built-in layouts and has a
"practice drifter while still typing on QWERTY" mode:

```sh
cargo install --git https://github.com/kayplicated/keywiz
keywiz -l drifter --from qwerty
```

That's the honest preview — you get to feel the alpha grid on
whatever keyboard you already have. The thumb cluster's
punctuation won't map cleanly to ANSI, but the 85% of typing
that happens on the alpha grid will. If the shape feels right
there, it'll feel much better on a real col-stag board.

### Daily use

Configure drifter in your board's firmware (QMK, ZMK, or
whatever your keyboard speaks). The alpha grid and thumb cluster
positions transfer directly; firmware-specific macros, combos,
and layer holds are your own design.

This repo intentionally doesn't ship firmware configs. Every
col-stag board has its own firmware syntax, and every typist has
their own combos, gaming layer, OS modifiers, home-row mods —
a canned config would embed assumptions that don't fit you.
Configuring drifter into your existing setup by hand is the only
way the firmware ends up matching how you type.

### Scoring

drifter's companion scorer [drift](https://github.com/kayplicated/keywiz/tree/master/drift)
accepts this format natively if you want to see the analyzer
breakdown:

```sh
drift score drifter.json
```

## Files

```
drifter/
├── README.md           — you are here
├── drifter.json        — the layout (keywiz JSON5 format)
└── docs/
    └── process.md      — iteration log from the v5 era
```

## License

AGPL-3.0, matching keywiz.
