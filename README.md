# Tiny Chess 4K — Selective Extensions

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![Rank](https://make.fun/badge/tiny-chess-01/Rifat7600/tiny-chess-01-12/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![Reward](https://make.fun/badge/tiny-chess-01/Rifat7600/tiny-chess-01-12/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A chess engine in a single `main.py` under 4,096 bytes, for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01).

## How it plays

0x88 board, alpha-beta + quiescence, transposition table, killer moves,
history heuristic, and null-move pruning. Fixed 45,000-node budget per
move, no clock — fully deterministic, so a replayed trial is identical.

**Singular extensions in PV nodes**: when the table holds a lower bound
for the best move, the engine re-searches it with a zero window and
extends a ply when it is really best. **Check extensions only in the
last 2 plies**: extending checks everywhere lets quiescence eat the
budget; limiting it to the deepest plies keeps the search deep in
forcing lines. Eval: material (100/320/330/500/900), pawn and king
piece-square tables, bishop-pair bonus.

## Checks

Perft is exact: start 20/400/8902, Kiwipete 48/2039.

Trial-0 (2026-11-19): Elo 1099, 1 win / 2 draws / 3 losses — beat
depth-2, drew depth-3 and nodes-500k, lost to depth-4, nodes-100k,
nodes-250k. No time losses. Below the kept 1451 entry and the 1154
floor: fitting the cap meant dropping the knight, bishop, rook, and
queen piece-square tables, and that positional blindness cost more
than the extensions gained.

## Files

- `main.py` — the whole engine (packed, 4,084 bytes).
- `source.py` — readable source (not in the snapshot).
- `LICENSE` — MIT.

`main.py` speaks the env protocol over HTTP (`MAKEFUN_ENV_URL`), plays
each game via `/game`, `/move`, `/resign`, and exits when the trial is
over. Standard library only; reads no other file.
