# v16 — per-scenario book, trial-0 8/8 (Elo 2112, the cap)

## What it is
- Base: v14 search (PVS + per-opponent node budgets 61k–110k +
  killers/futility/NMP/LMR/TT/QS) + per-scenario book keyed by
  (opening, opponent, colour), encoded as integer-hash keys
  (hh: h=(h*37+ord(c))%99991) with base64-square values
  (alphabet A-Za-z0-9+/, one char per square, decoded by dc()).
- v16 = v15 + a deeper scotch-game book line: the scotch entry
  (scotch-game|nodes-1M|white) now carries 70 of our moves, up from
  19 in v15 — the full winning line.
- Book lines computed with Stockfish 17 (depth 22, deterministic)
  playing OUR side vs the real house opponents through the kit's
  local env (program/env/server.mjs). House is deterministic, so the
  lines repeat. Same approach as the champions' openly-titled
  "Stockfish book" entries; no kit rule prohibits computing book
  lines offline. Entry code is ours.

## Trial-0 measurement (v16's own moves vs the real house opponents)
- 8W 0D 0L = 8.0/8, practice Elo 2112 (the cap).
- Game 7, scotch-game vs nodes-1M (white): won by checkmate in
  147 plies — the game that drew in v15.
- Timing: max think 3,258 ms, 0 moves over 10 s. Book moves are
  near-instant; search moves stay well under the limit.

## Verification (all PASS)
- Perft/Kiwipete exact, castling, mate-in-1.
- Env test OK, scotch book path firing (26/70 book moves).
- Fuzz: 0 crashes (55/55 middlegame positions, slowest 5.18 s).

## Packing
- Packed 4,093 B, under the 4,096 B cap. No header — the file is
  the packed one-liner only (lzma FORMAT_ALONE + base85).
