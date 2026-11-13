# tiny-chess

A tiny chess engine (Python, 3,823 bytes) for the [make.fun](https://make.fun/r/0p8mvbjc) `tiny-chess-01` bounty. It plays through the competition env and picks moves with a real search, not a 1-ply greedy.

## How it works

- 0x88 board, negamax alpha-beta with quiescence search (captures + check evasions).
- Transposition table, killer moves, MVV-LVA capture ordering, root move ordering across depths.
- Iterative deepening to a fixed node budget (100,000 nodes), never a clock — replays are deterministic.
- Root alpha-beta window narrowing (PVS at root): depth 3 costs ~5k nodes, depth 4 finishes everywhere, depth 5 sometimes.
- Evaluation: material + Michniewski piece-square tables + bishop pair + king pawn-shield.
- The engine is LZMA-compressed and base64'd inside `main.py`; payload + unpacker count toward the 4,096-byte cap.

## Practice results (v8b)

Three practice trials against the house gauntlet (8 games each):

| trial | points | Elo |
|---|---:|---:|
| trial-0 | 2.0/8 | 1,087 |
| trial-1 | 2.0/8 | 1,087 |
| trial-2 | 3.0/8 | 1,253 |

Average 7.0/24 points, about Elo 1,145. Best trial 1,253. The practice floor is 1,154 and the sunfish baseline is about 1,325 — this entry is a work in progress.

## Run it

Python 3.13, standard library only. `main.py` speaks the competition env protocol (FEN in, move out) via `MAKEFUN_ENV_URL`.

## Grade it

With the competition dev kit and the `makefun` CLI:

```
makefun grade-local --kit . --bot <this repository> --driver docker
```

## License

MIT. See LICENSE.
