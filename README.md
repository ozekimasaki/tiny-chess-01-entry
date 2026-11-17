[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ozekimasaki/tiny-chess-01-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ozekimasaki/tiny-chess-01-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01 entry: a 4 KB chess engine

`main.py` is the whole engine: at most 4,096 bytes as committed,
Python 3.13, standard library only. It plays every game through the
env (POST /game, POST /move) and never reads another file.

## How it works

The readable source is lzma-compressed and base85-encoded inside
`main.py`, which decompresses and execs it (compression is allowed;
the payload and the unpacker both count toward the 4,096 bytes).

Inside is a complete negamax engine:

- Bitboard-free mailbox board (64 squares) with incremental
  Zobrist hashing and incremental material + piece-square evaluation
  (classic PSTs packed as mirrored half-tables).
- Principal-variation search with a transposition table, null-move
  pruning, late-move reductions, check extensions, and MVV-LVA move
  ordering.
- Quiescence search over captures and promotions, with full
  check-evasion search when in check.
- Repetition and fifty-move draws are scored 0 inside the search,
  seeded from the game's full move history.
- Deterministic: iterative deepening under a fixed node budget
  (100,000 nodes per move), never a clock, no randomness outside a
  fixed-seed Zobrist table.

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed.
- The whole repository snapshot is at most 8,192 bytes, no file
  over 4,096. MIT licence.
