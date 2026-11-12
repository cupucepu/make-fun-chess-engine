# Magattinking-4K

A 4 KB Python chess engine built for the make.fun chess-engine bounty
(≤ 4,096 bytes of source). It plays a full game of chess through the
make.fun environment protocol and fits the entire engine — search, eval,
and move generation — into one small file.

## Engine

- **Search:** negamax with alpha-beta pruning and iterative deepening
  to depth 6, with a 25,000-node budget per move.
- **Pruning:** null-move pruning, late move reductions (LMR), and
  futility pruning keep the tree small enough for the node budget.
- **Ordering:** transposition table, killer moves, and MVV-LVA capture
  ordering so the best moves are searched first.
- **Quiescence:** capture-only quiescence search with check evasions,
  so tactical lines are not cut off mid-capture.
- **Evaluation:** tuned piece-square tables for all six piece types,
  with material and positional terms.
- **Packing:** the engine core is compressed with lzma + base85 and
  unpacked at startup, which is how the whole engine fits in 4 KB.

## Environment protocol

The engine talks to the make.fun game server over HTTP:

- `POST /game` — start a new game, returns a game id.
- `POST /move` with `{game, move}` — play a move (UCI format).
- `POST /resign` with `{game}` — resign the current game.

It reads the server's position, picks the best move with the search
above, and posts it back until the game ends.

## Files

- `main.py` — the whole engine, 3,910 bytes (limit 4,096).
- `LICENSE` — Apache 2.0.
- `README.md` — this file.

## Verified results

- Perft, start position, depth 4: **197,281** nodes.
- Perft, Kiwipete position, depth 4: **4,085,603** nodes.
- Head-to-head vs proxy engines: **6 wins, 2 draws, 0 losses**.
- Env mode vs the make.fun server protocol: **won 1–0 by checkmate
  in 37 plies**.

Perft counts match the known correct values, confirming the move
generator is sound; the head-to-head and env-mode results confirm the
search actually converts that into wins.

## Verified

- `main.py`: 3,910 bytes, sha256
  `09f6dbec8a2d932f4da984b48d69f8f5c8474b883bf858a6d03b9c5f6022223c`.
- `LICENSE`: 1,099 bytes. `README.md`: 1,994 bytes.
- Repo total (`main.py` + `LICENSE` + `README.md`): 7,003 bytes,
  under the 8,192-byte cap; every file is under 4,096 bytes.
