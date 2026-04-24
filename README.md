# drifter

A flexion-biased col-stag keyboard layout. Designed on a Halcyon
Elora v2, drilled through seven iterations, built around two
biomechanical observations: **row direction is asymmetric on
aggressively-staggered col-stag hardware**, and **hand-territory
synchrony — whether both hands reach the same rows during
alternation — is a felt axis that generic scorers don't name.**

```
alpha grid:
  q v m j [    ] - = x z
  n r t s g    p h e a i
  b l d c w    k f u o y

right thumb cluster (k3..k6):
                    ;  '  ,  .
```

The top row is low-traffic: rare letters (`q v m j x z`) plus
the ANSI-outer punctuation (`[ ] - =`) that doesn't fit
elsewhere. All frequent punctuation lives on the thumb cluster.

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

**Dense local rollability.** The right-hand bottom cluster
`k f u o y` is packed with rollable pairs in multiple
directions: `of`/`fo`, `ou`/`uo`, `ko`/`ok`, `you`/`uoy`,
`oy`/`yo`, `ku`/`uk`. Common bigrams roll cleanly both inward
and outward through the same zone. The hand never leaves the
region to type most common words; it *circulates* through
different adjacent-finger pairings within the cluster. The
feeling I chased across iterations — and named the layout for —
is drifting over the keys.

**All frequent punctuation on the right thumb cluster** (`; ' , .`).
The Elora's 7-key thumb cluster makes thumb-reach cheap enough
that even mid-word characters like `'` work well there —
contractions (`don't`, `it's`, `they're`) flow through the thumb
without interrupting alpha rhythm, and freeing `'` off the alpha
grid shortens the common `i'` reach from pinky-to-index-outer-top
to pinky-to-thumb. `; , .` never appear mid-word in prose anyway,
so their thumb placement is free.

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

## Installing

### Via keywiz (typing practice)

[keywiz](https://github.com/kayplicated/keywiz) ships with
drifter as one of its built-in layouts:

```sh
cargo install --git https://github.com/kayplicated/keywiz
keywiz -l drifter
```

If you're coming from QWERTY and want to practice drifter while
still typing on a QWERTY-mapped keyboard:

```sh
keywiz -l drifter --from qwerty
```

### Via firmware

drifter.json is a data-only description — to actually type on it
you need firmware for your board. The repo ships with format
adapters where available:

- **kanata** — `kanata/drifter.kbd` in this repo is an ANSI
  adaptation for users who want to practice drifter muscle memory
  on a standard keyboard. It folds the thumb cluster's `; ' , .`
  into the alpha top row's otherwise-quiet slots. See the file
  header for the compromise details.
- **QMK / ZMK** — not in the repo yet. Split col-stag users are
  better off configuring drifter directly in their board's
  firmware, since the thumb cluster is the point and a kanata-on-
  ANSI config can't preserve it.

### Via scoring (drift)

drifter's companion scorer [drift](https://github.com/kayplicated/keywiz/tree/master/drift)
accepts this format natively:

```sh
drift score drifter.json
```

## Files

```
drifter/
├── README.md              — you are here
├── drifter.json           — the layout (keywiz JSON5 format)
├── kanata/
│   └── drifter.kbd        — ANSI-adapted kanata config
└── docs/
    └── process.md         — iteration log from the v5 era
```

## License

AGPL-3.0, matching keywiz.
