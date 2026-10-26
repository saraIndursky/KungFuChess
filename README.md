# Kung Fu Chess

A real-time, multiplayer version of chess with no turns: both players move whenever they want.
A moving piece travels across the board over time, and after it lands it rests for a short
cooldown before it can move again. Pieces can also jump in place.

Built in Python with a server-authoritative architecture: the server owns the game state,
and clients only send move requests and render the snapshots they receive.

## Features

- **Real-time rules:** timed movement, cooldowns, jumps, and collision resolution when two
  pieces reach the same square.
- **Full chess piece rules:** king, queen, rook, bishop, knight and pawn, including pawn promotion.
- **Multiplayer over WebSockets:** a game server that hosts matches, accepts players, and
  broadcasts game snapshots and motion events to every client.
- **Client-side motion interpolation:** clients animate moving pieces smoothly between server
  updates and reconcile with the server state.
- **Graphical UI (OpenCV):** animated sprites, move history for each player, score panels,
  board coordinates and a promotion picker.
- **573 automated tests** (unit and integration) covering rules, engine, networking, snapshots and UI.

## Architecture

```
kungfu_chess/
  model/       Board, pieces, positions
  rules/       Move validation for each piece type
  engine/      Game engine: moves, jumps, captures, collisions, promotion
  realtime/    Motion, movement duration, state timers
  events/      Event bus, messages and handlers
  scoring/     Piece values and score tracking
  history/     Move history
  snapshot/    Immutable game snapshots for rendering and networking
  server/      Game server, matches and player sessions
  network/     WebSocket server and client, snapshot serialization
  client/      Network client logic
  input/       Mouse input and click routing
  ui/          Rendering, animation and layout (see ui/README.md)
  view/        Text views
tests/
  unit/        Tests for each package
  integration/ End-to-end game flows
```

The layers depend in one direction only: the UI depends on the engine, never the other way
around, and the renderer draws from immutable snapshots without touching game logic.

## Requirements

- Python 3.12 or newer

```bash
pip install numpy opencv-python websockets pytest
```

## Running

Local game on one computer:

```bash
python -m examples.game_app_demo
```

Multiplayer over the network. Start the server, then one client for each player:

```bash
python -m kungfu_chess.demos.server_demo
python -m kungfu_chess.demos.client_demo player1
python -m kungfu_chess.demos.client_demo player2
```

The server listens on `localhost:8765`.

## Tests

```bash
python -m pytest
```
