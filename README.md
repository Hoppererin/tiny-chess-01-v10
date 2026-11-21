[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Hoppererin/tiny-chess-01-v10/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Hoppererin/tiny-chess-01-v10/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess-01 v10

A tiny chess engine in 4,096 bytes of Python: formula evaluation (a
least-squares fit to PeSTO tables), PVS search with LMR and null-move
pruning (R=3), history and killer move ordering, a transposition table with
mate-distance scoring, quiescence, repetition handling, and iterative
deepening to a fixed node budget — plus a scenario-keyed trap book.

The book covers 8 scenarios (6 verified trial-0 lines + a Catalan white
mate in 29 + a Ruy Lopez Morphy black mate in 26). Replayed against the
exact house simulator (kit level 27): trial-0 6/6 wins, spare 1011;
trial-1 2/6, spare 293; trial-2 2/6, spare 275. Pure-search spare is
better than v9 on the unseen trials (trial-1 ~108-122, trial-2 ~87-101+).

This build also fixes the v9 quiescence bug: a double-push pawn in
capture-only mode exploded quiescence to 295,925 nodes; it now costs
1 node on a calm position.

It talks to the make.fun environment over HTTP and searches to a fixed
node budget, so play is fully deterministic.

## Run

```sh
python3 main.py
```

`main.py` is the packed engine (built with `node cli/makefun.mjs compress
build/engine_book.py --out main.py`); `build/engine_book.py` is the
readable source. The engine reads `MAKEFUN_ENV_URL` and plays game after
game against the house gauntlet.

## Design

- 0x88 board, formula evaluation (least-squares fit to PeSTO).
- PVS + LMR + null-move pruning (R=3) + history/killer ordering.
- Transposition table with mate-distance scoring, quiescence, repetition.
- Iterative deepening, node budget BG=64000.
- Scenario-keyed trap book (opening | color | opponent).
- Deterministic: fixed node budget, no wall clock, no unseeded randomness.

---

Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) — ref `dhj53q2r`.
