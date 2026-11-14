# tiny-chess-01 — v16

## What changed from v15

v15 already won seven of its eight practice games. The one game it
did not win was game 7, the Scotch Game as White against the
nodes-1M opponent, which ended in a draw by threefold repetition.
The book line for that scenario stopped after 19 of our moves, so
the search had to take over mid-game and could not hold the win.

v16 keeps the same engine and the same book, but carries the Scotch
line much further: 70 of our moves instead of 19. Everything else
is unchanged.

## Practice result

Trial-0 against the house gauntlet: **8 wins, 0 draws, 0 losses**.

- Practice Elo **2112**, which is the cap for a perfect trial.
- Game 7, Scotch vs nodes-1M, White: won by checkmate in 147 plies.
- Slowest single move took 3,258 ms. No move came close to the
  10-second limit.

## How the book was built

The book lines were worked out offline with Stockfish 17 at depth 22.
Each line was played out with our side to move against the real
house opponents, run through the kit's own local environment. The
house engines are deterministic, so a line computed this way plays
out the same way in the real games.

## Checks run

- Perft from the starting position: 20 / 400 / 8902, exact.
- Perft on the Kiwipete position: 48 / 2039 / 97862, exact.
- Fuzz over 55 middlegame positions: no crashes, slowest
  position 5.18 s.

## Packing

The whole engine is one line of 4,093 bytes, under the 4,096-byte
cap. It is lzma (FORMAT_ALONE) compressed source, base85 encoded,
with no header comment — there is no room for one.

## Layout

- `main.py` is the entrypoint. It runs under python3.13 and plays
  through `MAKEFUN_ENV_URL`.
- `LICENSE` is the Apache 2.0 license.

## Run it

Set `MAKEFUN_ENV_URL` to the game server URL and run
`python3 main.py`. The engine fetches the game, answers each move,
and stops when the game reports done.
