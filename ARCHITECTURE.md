# Architecture

This document is the **intended** shape of AirPods Bubble. None of the runtime pieces exist yet. Folder names under `App/` are reserved so later work stays modular.

If implementation disagrees with this file, update this file in the same change. Do not leave the doc claiming a pipeline that the code does not have.

## App modules

```text
App
├── Audio         # capture, convert, play; no networking
├── Networking    # browse, listen, connect, send/receive bytes
├── Security      # pairing, identity, TLS configuration
├── UI            # screens and user-facing state
└── Utilities     # shared helpers with no audio or network policy
Tests             # tests per module; no “the whole bubble works” test until Milestone 10
```

Rules:

- Audio must be usable in a single-phone loopback test (Milestone 2) without Networking.
- Networking must move dummy payloads without Audio (Milestones 3–4).
- Security must be able to refuse an unauthenticated peer (Milestone 9) without depending on UI chrome.
- UI may orchestrate modules. It must not embed capture or socket logic.

## Proposed audio pipeline

**Status: proposal. Not implemented.**

```text
AirPods microphone   (system route; not guaranteed)
        ↓
AVAudioEngine / AVAudioInputNode
        ↓
PCM
        ↓
processing / encoding   (format TBD; public APIs only)
        ↓
Networking (bytes on NWConnection)
        ↓
decoding
        ↓
PCM
        ↓
AVAudioPlayerNode
        ↓
AirPods                (system route; not guaranteed)
```

The reverse path is the same on the other phone. Two-way means two of these pipelines at once. That is Milestone 7, not a day-one object.

## Proposed networking

**Status: proposal. Not implemented.**

```text
NWListener + Bonjour advertise
        ↓
NWBrowser finds Bubble peers
        ↓
pair / authenticate     (Milestone 9; not optional for a later release)
        ↓
NWConnection
        ↓
bidirectional audio frames
```

Transport protocol (TCP, QUIC, or framed UDP) is undecided. Choose it when Milestone 4 exists and can be measured.

Do not introduce Multipeer Connectivity as the core design.

## Security

**Status: proposal. Not implemented.**

Milestone 4–7 may use a development-only trust path if that is required to get two phones talking, but that path must be labeled **insecure / development only** in code and docs. Milestone 9 replaces it with documented TLS and an explicit pairing step.

## What this architecture does not include

- Private Apple APIs
- A claim that AirPods, ANC, or Bluetooth profiles will obey the diagram
- A single class that captures, encodes, connects, and plays
- Nearby Interaction as the audio transport
