# Tiny Chess 01 — make.fun entry

[![Bounty](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01) [![Rank](https://make.fun/badge/tiny-chess-01/Ryota537/makefun-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01) [![Reward](https://make.fun/badge/tiny-chess-01/Ryota537/makefun-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A compact, deterministic chess engine in a single `main.py` (4,014 bytes,
LZMA-packed). Standard library only; no network except the make.fun
game environment.

## Engine

0x88 board, piece lists with O(1) make/unmake, Zobrist hashing.
Negamax alpha-beta, iterative deepening, fixed total node budget
(90,000 nodes/move, ~2 s/move). Transposition table (capped at
400,000 entries), 2 killers/ply, history heuristic, MVV-LVA +
promotion + killer + history ordering. Null-move pruning (R=2/3,
only when static eval ≥ beta and the side has non-pawn material).
No LMR — it was removed after it missed a forced mate (fxg4 scored
+226 instead of mate in 3). Quiescence: stand-pat, captures/
promotions, full evasions when in check. Eval: material + formula
piece-square tables + tapered king table + bishop pair. Resigns at
score ≤ −2500.

## Local results (kit env, real house engines)

| trial | result | Elo |
|---|---|---|
| practice trial-0 | 4W 0D 2L | 1451 |
| practice trial-1 | 3W 1D 2L | 1364 |
| 8-game final sim | 4W 0D 4L | 1396 |

8-game sim: wins vs depth-2, depth-4, nodes-100k, nodes-250k (all
checkmate); losses vs depth-3, nodes-500k, nodes-1M, nodes-2M.
Per-move: median 1.9 s, max 3.2 s (limit 10 s). 8-game wall 670 s
(limit 1800 s). Sunfish 4K baseline 8-game mean ≈ 1325.

## Files

- `main.py` — the entry (packed); the only file the grader runs.
- `engine.py` — readable source, **not committed on purpose** (snapshot
  cap is 8,192 bytes). Rebuild with
  `node <kit>/cli/makefun.mjs compress engine.py --out main.py`
  (own command, after the last source change).
- `LICENSE` — MIT.
