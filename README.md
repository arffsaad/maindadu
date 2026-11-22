# Dadu

A multiplayer dice betting game in a single `index.html`.

- **GitHub Pages:** https://arffsaad.github.io/maindadu/ (multiplayer over WebRTC via PeerJS; the free PeerJS cloud server is only used for matchmaking)
- **Claude artifact:** https://claude.ai/artifact/JNtoXzqqQdAatWfZY4Gtdr (multiplayer via the artifact `room` capability)

## Rules
- Everyone starts with 200 coins; each round costs 20.
- Each player picks one number on the die; no two players may share a number.
- When everyone has picked, the die is rolled. A match takes the whole pot; otherwise the pot carries over.
- Can't cover the bet and you're out. Last player standing wins.

## How multiplayer works
The room creator's browser is the game host and keeps the game state. Outside claude.ai, browsers connect
peer-to-peer with PeerJS: one browser claims the hub id `dadu-hub-<room code>` and relays everyone's state.
If the host leaves, the remaining players re-elect a hub and a new game host, keeping coins and the pot.

Debug the network layer by running `window.DADU_DEBUG = 1` in the console before joining.
Point at your own PeerJS server with `window.DADU_PEER_OPTS = {host, port, path, secure}`.
