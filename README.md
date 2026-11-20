# P2P Flat Messenger

A serverless messenger that runs entirely in the browser. No backend, no accounts, no
database — you get a peer ID, send it to the other person, and the two browsers talk
directly over WebRTC (PeerJS handles the signalling).

Open `index.html` (or host the folder anywhere static) and you are online.

## How it works

1. **My ID** — on first load PeerJS generates a peer ID, stored in `localStorage` and reused
   on the next visit. If the ID is already taken in the signalling network, a new one is made.
2. **Connect to Peer** — paste the other side's ID and hit connect.
3. Messages go over the direct data channel; history per peer and the list of saved
   connections stay in `localStorage` — nothing leaves the device.
4. **Voice messages** — recorded with `getUserMedia`, encoded as `audio/webm`, capped at 5 MB.

## Files

```
index.html      UI shell (sidebar: my ID, connect, active connections)
style.css       flat dark UI, Manrope
js/main.js      boot: initUI → initMedia → initPeer
js/peer.js      PeerJS setup, connection lifecycle, reconnect by saved ID
js/state.js     shared state (peer, connections, messages)
js/storage.js   localStorage: my id, saved connections, per-peer history
js/media.js     voice message recording
js/ui.js        rendering of the connection list and chat
```

## Notes

- Requires network access for the PeerJS signalling server; the messages themselves are P2P.
- No persistence beyond the browser: clearing site data removes the ID and the history.
- There is no encryption layer of its own — WebRTC data channels are DTLS-encrypted in transit,
  but this is a chat toy, not a hardened messenger. Do not use it for secrets.