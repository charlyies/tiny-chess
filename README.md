# tiny-chess-01: a 4 KB chess engine

`main.py` is the whole engine (3,771 bytes as committed): a compressed
payload (`makefun compress` LZMA stub) of a readable Python source kept
beside the entry at bundle time. Python 3.13, standard library only; it
reads no other file and calls no service but the env at `MAKEFUN_ENV_URL`.

## The engine

- 0x88 board, in-place make/unmake, pseudo-legal generation filtered by
  king safety (castling, en passant, promotions, all verified by perft).
- Negamax alpha-beta with a transposition table, killer moves and
  MVV-LVA ordering; quiescence over captures, promotions and replies to
  check, with delta pruning; null-move pruning (R=2, never in check or
  without a non-pawn piece); a position that repeats in the game or the
  search path scores 0.
- Iterative deepening to a fixed node budget per iteration (depth 1 is
  unbounded; an iteration that runs past the budget is dropped whole and
  the last whole depth's move stands) — the same structure as the house
  gauntlet engines. No clock, no randomness: replays play the same moves.
- Evaluation: material plus piece-square terms and the bishop pair.
- Root moves are re-sorted by the last iteration's scores with a stable
  sort, so the last whole depth's best move stands among ties.

## Local results (practice trials vs the house gauntlet)

- trial-2: 2 wins, 0 draws, 4 losses (beat depth-2 and depth-3) — Elo ~1100.
- trial-0 (this entry, compressed): 1 win, 1 draw, 4 losses.
- Perft: all six standard positions exact through depth 3–4.

## Licence

MIT, see LICENSE.
