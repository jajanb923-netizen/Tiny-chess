# tiny-chess-01

My entry for "Build the strongest chess engine in 4 KB of Python".

## Engine

`main.py` (4,093 bytes, Python 3.13 stdlib only) is a self-contained
chess engine that unpacks and execs a compressed payload. It talks to
the house env over HTTP and replies in UCI.

- 0x88 board, incremental evaluation (material + piece-square tables,
  tapered king safety) and Zobrist hashing.
- Alpha-beta search with quiescence (captures, promotions, all replies
  when in check), MVV-LVA + killer move ordering, and a transposition
  table (exact / lower / upper bounds).
- Null-move pruning (R=2) and threefold-repetition detection.
- Iterative deepening to a fixed node budget of 180,000 nodes per move
  (never a clock), so replays are deterministic. No model calls.

## Size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed.
  It reads, imports or execs no other file in the repository.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes
  in all, and no file over 4,096. MIT licence.

## Grade it locally

You need the `makefun` CLI and the competition's dev kit. From inside
the kit:

    makefun grade-local --kit . --bot <your repository> --driver docker

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it. Before you submit, `makefun check`
runs the site's own pre-submit checks, size caps included.
