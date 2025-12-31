# static-p2p-pong

A small [live demo](https://katehuuh.github.io/static-p2p-pong/) - Serverless P2P Rollback Netcode running on static GitHub Pages via WebRTC (No Cloudflare, Firebase, Supabase, or db PHP, just PeerJS). Inspired by [mitxela's WebRTC, UDP Hole Punching via a STUN server Pong](https://mitxela.com/projects/webrtc-pong).

## ⚡ Mechanics & Architecture
- **Solves Static-hosting Matchmaking limitation by using Distributed Recursive Lobbies:** Recursive logic -> Host `Room_N` -> (Busy?) -> Join `Room_N` -> (Timeout?) -> Retry `Room_N+1`.
- NAT Traversal: STUN `stun.l.google.com` -> Public IP -> PeerJS Signaling -> UDP Hole Punching -> Direct DataChannel.
- Determinism: Physics rely on integers to prevent desynchronization, predicting and rolling back action when needed:
  - Rollback: Inputs apply instantly (0ms lag) -> Rewinds time when remote data arrives -> Replays physics -> Fast-forwards.
  - Sync: Speed up or slow down the game clock slightly to keep players together (Drift Correction).
- 📦 Modules Structure: `NetworkManager`: PeerJS lobby & connection, `RollbackEngine`: Records history snapshots + Rewind logic, `PongGame`: Integer physics & UI.
