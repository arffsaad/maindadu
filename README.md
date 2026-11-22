# Dadu

A multiplayer dice betting game in a single `index.html`.

- **GitHub Pages:** https://arffsaad.github.io/maindadu/ (multiplayer through free public MQTT brokers over secure websockets, so it works on mobile data)
- **Claude artifact:** https://claude.ai/artifact/JNtoXzqqQdAatWfZY4Gtdr (multiplayer via the artifact `room` capability)

## Rules
- Everyone starts with 200 coins; each round costs 20.
- Each player picks one number on the die; no two players may share a number.
- When everyone has picked, the die is rolled. A match takes the whole pot; otherwise the pot carries over.
- Can't cover the bet and you're out. Last player standing wins.

## How multiplayer works
The room creator's browser is the game host and keeps the game state. Outside claude.ai, every player
publishes its state as a retained MQTT message on `dadu/v2/<room code>/p/<player>` through two public
brokers at once (`broker.emqx.io`, `broker.hivemq.com`), and a last-will message clears it when a player
drops. If the host leaves, the remaining players elect a new host, keeping coins and the pot.

The public brokers are shared and unauthenticated: anyone who knows a room code could read or spoof its
messages. Fine for a casual game; don't put anything private in it.

Debug the network layer by running `window.DADU_DEBUG = 1` in the console before joining.
Use your own brokers with `window.DADU_BROKERS = ['wss://host:port/mqtt', ...]`.
