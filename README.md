# SpeedChess

A tiny phone-first realtime chess game for two trusted players.

## MVP

- Create a private-ish 6-character room code
- Share the code or invite link with the other player
- White hosts; Black joins
- Realtime moves over a browser-to-browser WebRTC data channel
- 1, 3, or 5 minute clocks
- Legal move validation
- Checkmate, stalemate, repetition, insufficient-material, and timeout results
- Mobile-friendly board with automatic Black orientation
- No accounts and no application backend

## How it works

The site is static and can be hosted entirely on GitHub Pages.

- [chess.js](https://github.com/jhlywa/chess.js) handles chess rules and legal moves.
- [PeerJS](https://peerjs.com/) provides the WebRTC data connection.
- PeerJS Cloud is used only to broker the initial connection. Game messages then travel over the WebRTC data channel.
- The 6-character room code is used to derive the host's PeerJS ID. For this MVP we assume both players are friendly/trusted and possession of the room code is sufficient authorization.
- The host is authoritative for legal moves, clock state, and the result.

No persistent game data is stored.

## Play locally

Because the app uses browser ES modules, serve the directory instead of opening `index.html` directly.

For example:

```bash
python -m http.server 8080
```

Then open `http://localhost:8080`.

For two real phones, use the GitHub Pages deployment so both devices have HTTPS.

## GitHub Pages

A Pages deployment workflow lives at `.github/workflows/pages.yml` and deploys the repository root on every push to `main`.

If Pages has not yet been enabled for this repository, do the one-time setup:

1. Repository **Settings**
2. **Pages**
3. Under **Build and deployment**, choose **GitHub Actions**

The expected URL is:

`https://andrewboudreau.github.io/SpeedChess/`

> Note: the repository can remain private on GitHub Pro, but a normal GitHub Pages site is publicly reachable. The game itself contains no stored data; joining requires knowing the room code.

## MVP limitations

- Promotion automatically chooses a queen.
- No spectators, chat, takebacks, persistence, matchmaking, ratings, or authentication.
- Peer-to-peer connectivity can fail on restrictive networks that require a TURN relay. A later version can add a tiny relay/signaling service if that becomes an issue.
- The host tab is the clock authority, so the host should keep the game page open during play.

## Next ideas

- Promotion picker
- Rematch button
- Increment controls (3+2, 5+3)
- Move list / PGN
- Sounds and haptics
- Installable PWA
- Optional self-hosted signaling/TURN service
