# Drifter — design process, v5 era

> **Historical note.** This is a session log from the drifter-v5
> era, back when the question was still "is drifter actually
> better than gallium?" The layout has since gone through v6, v7,
> and the now-shipped form — see the main [README](../README.md)
> for the current layout and rationale. Kept here as process
> reading: how the thinking evolved, what tools were reached for,
> what "gallium v2" meant at the time (the col-stag variant now
> called gallium-qxjz). The "don't keep iterating" advice at the
> bottom of the TL;DR is the thing I then promptly ignored — and
> the layout got better for it.

A long session through oxey-assisted layout design for a Halcyon
Elora v2 (col-stag, choc) plus personal corpus (~1.1M chars of
Claude Code chatlogs).

## TL;DR

- **drifter-v5** is the layout to drill. Mechanical flip of gallium
  v2 with hand-tuned tweaks. Home row unchanged. Top-row frequent
  letters moved to bottom.
- Oxey score essentially tied with gallium on Kay's corpus. Ergonomic
  difference is a *direction* bet (flexion vs extension), not a
  model-measurable win.
- Every manual swap we tried to "improve" v5 made oxey's score
  worse. v5 is a local optimum; drilling is the only way to judge
  further.
- **Don't keep iterating.** Drill v5 for a week, then decide.

## The layouts

Stored in `keywiz/layouts/` as `drifter-v{1..5}.json`, accessible
at runtime via `Ctrl+←/→` or `-l drifter-vN`.

**gallium v2** (baseline):
```
b l d c v    z y o u ,
n r t s g    p h a e i
q x m w j    k f ' ; .
```

**drifter-v5** (the one to drill):
```
q x m w j    z k ' ; ,
n r t s g    p h a e i
b l d c v    y f o u .
```

- Left hand: full top↔bot row flip from gallium.
- Right hand: middle and ring columns flipped top↔bot; y↔k cross-
  column swap; pinky column and index-inner (`z`, `,`) unchanged.
- Home row identical to gallium v2.

Earlier variants (v1–v4) are documented in the json headers but
most have been superseded by v5. v4 is the closest alternative if
v5 ends up unfavored.

## What this session actually built

### Oxey patching

Oxey is vendored at `oxeylyzer/` with a path-dep override on libdof.

- **libdof**: added `FormFactor::Elora` with DXF-derived geometry
  from the splitkb Elora v2 top plate. Covers 30-key alpha core +
  12-key thumb cluster with real column stagger.
  (`oxeylyzer/libdof-patched/src/{dofinitions,keyboard}.rs`)
- **oxey scissor threshold**: changed from 1.9 → 0.8 for col-stag.
  The old 1.9 threshold was tuned for ortho/row-stag boards where
  dy between adjacent rows = 1.0 and only row-skipping (dy ≥ 2.0)
  registers as scissor. On col-stag, forward-thrust middle column
  puts adjacent-finger same-row pairs at dy ~0.6, while real cross-
  row motions can dy ~1.0-1.5. 0.8 catches real scissors without
  false-flagging in-row stagger.
  (`oxeylyzer/oxeylyzer-core/src/fast_layout.rs`, line ~520)
- **row_extension_penalty weight**: new per-keystroke penalty for
  keys in the top alpha row. Captures biomechanical "reaching up
  recruits arm/shoulder" cost that Euclidean distance doesn't
  model. Negative values penalize. Usage: config.toml
  `row_extension_penalty = -1.0` to `-3.0`.

### Kay's personal corpus

- `oxeylyzer/static/language_data/kay.json` — extracted from
  `~/.claude/projects/-home-kay-lab*/**/*.jsonl` user messages.
  1.09M chars, 44 chars in scope (matching shai's set).
- Significantly different from shai prose: lower `,` (0.43% vs
  0.96%), higher `'` (0.76% vs 0.43%), higher `?` — because
  chat/code mix differs from novels/news.

## Why drifter-v5 is a local optimum

Every hand-swap attempted against v5 scored worse:

| Swap | Problem it introduced |
|------|-----------------------|
| c↔v (within LI) | `sc` SFB unchanged, more stretches |
| c↔d (c on LM) | `ct/tc` SFB at 0.48% — worse than `sc` |
| c↔b (c on LP) | pinky overload; `nc/cn` new SFB |
| c↔x (c on LR) | `cr/rc`, `cl/lc` — worse combined |
| c↔l (c on LR) | `rc/cr` SFB + `tc/ct` becomes big scissor |
| c↔`;` (c on RR) | `ce/ec` catastrophic SFB at 0.9% |
| c↔`,` (c on RP) | `ci/ic` SFB at 0.46% |
| c↔middle-R | `co/oc` disaster |
| g↔c | lsb explosion, stretches worse |

The pattern: `c` at 3.17% in Kay's corpus pairs heavily with
vowels (`co, ca, ci, ce`) and consonants (`ch, ct, cr, cl, ck,
sc, nc`). Every placement creates *some* bigram problem. Gallium's
placement at LI-next-to-`s` (giving `sc` SFB at 0.22%) is
empirically the minimum-cost location across all tested options.

## Hypotheses being tested by drilling

1. **Bottom row is biomechanically nicer than top row.** Flexion
   (curl down) recruits flexor tendons; extension (reach up)
   recruits extensors + often the whole arm forward. Kay's
   subjective sense, not captured by oxey's distance-based scoring.
2. **Same-row adjacent-finger rolls beat cross-row diagonals.**
   E.g. `fo` where both letters are bot-row adjacent fingers
   (v4_yf) is argued to feel better than `fo` where `f` is home
   and `o` is bot (v4_candidate), even though oxey's scissor
   metric misses some of this distinction.

Oxey cannot judge either hypothesis — the scoring is geometric
and bigram-pattern based, not biomechanical. Drilling is the only
way to get data on these.

## Blind spots in oxey (as patched)

- **Lateral-twist moves not captured.** Cross-row adjacent-finger
  motions with significant dx component (e.g. home-R-inner to bot-
  R-middle) feel like twists, not just scissors. Oxey measures
  dy-scissor and nothing else.
- **Hand-shape recovery cost not modeled.** Oxey measures key-to-
  key distance, not "does my hand have to leave its home position
  to type this." So two letters that each take a finger far from
  home can score as cheap if they're close to each other, even
  though the hand is in a compound position.
- **Oxey's finger weights are inverted.** Higher weight = cheaper
  to move. Lower weight (pinky=1.4) = more expensive. This means
  oxey is aggressive about putting letters on index/middle
  (high weight = cheap to move) and conservative on pinky.
- **Scissor detection depends on keyboard geometry.** Fix applied
  (0.8 threshold) works for Elora col-stag but hand-calibrated.
  Row-stag boards need 1.9; ortho ~1.0-1.5.

## Files produced

```
keywiz/
  layouts/
    drifter-v1.json   # raw oxey output, weird
    drifter-v2.json   # oxey + row penalty, home pinned
    drifter-v3.json   # mechanical column flip
    drifter-v4.json   # oxey with y↔f fix (close alternative to v5)
    drifter-v5.json   # ← drill this one
  docs/
    drifter-session.md  # this file
oxeylyzer/
  libdof-patched/     # vendored libdof with Elora form factor
  oxeylyzer-core/src/weights.rs      # row_extension_penalty
  oxeylyzer-core/src/fast_layout.rs  # scissor threshold 0.8
  oxeylyzer-core/src/generate.rs     # scoring integrates the new terms
  static/language_data/kay.json      # Kay's corpus
  static/language_data/kay_thumb.json # corpus with . and , removed (thumb-cluster variant)
  static/layouts/english/            # kay_* layouts (experimentation set)
  config.toml                        # row_extension_penalty = 0.0, corpus=kay.json
```

## Resumption notes for future sessions

- Config is left at `corpus = kay.json` and `row_extension_penalty
  = 0.0` (default, no penalty). To re-run with row penalty, set
  to -1.0 or -3.0 and regenerate.
- Scissor threshold is 0.8 in `fast_layout.rs` — correct for Elora.
  If analyzing a row-stag board, restore to 1.9.
- Layout `gallium v2 elora` uses the Elora form factor but with
  gallium v2's letter arrangement, for direct comparison.
- `kay_v4_yf` in oxey = `drifter-v5` shape (confusing naming —
  the keywiz port is the authoritative version).
- Oxey REPL pins can't accept single-quote in pin string due to
  lisp-parser quirks. Workaround: skip `'` from pin list and
  accept it as free.
- The `bld cv / yfo u.` right side reads "fck" on bottom. Noted.

## Decision log

- Attempted flips: full row flip (breaks bigram structure), middle-
  only flip, middle+ring+index-outer flip (v3), oxey-optimized with
  row penalty (v2). Arrived at v5 as hand-tuned column flip +
  punctuation placement.
- Attempted thumb-cluster punctuation: removes `.` and `,` from
  alpha grid, score improves by ~0.02 but requires firmware
  changes to actually bind. Deferred.
- Attempted engram-style punctuation on inner columns: catastrophic
  SFB explosion because inner columns are on index finger which is
  already crowded. Rejected.
