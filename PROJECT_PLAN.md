# Project plan

AirPods Bubble is built in small milestones. Only one milestone is active at a time. A milestone is not done until its success test in [TESTING.md](TESTING.md) has been run, or until it is documentation-only (Milestone 1).

This file is the plan. It does not claim unfinished work is complete.

## Current constraint

No Mac is available. **Xcode, the iOS app target, and Milestones 2–10 are parked.** Do not scaffold an Xcode project or write Swift “just in case.”

Work that is still allowed on Windows: keep the docs honest, refine research and architecture, and plan tests. That is not a substitute for device proof.

## In progress / done

### Milestone 1 — Project setup

**Status:** done as of 0.1.0 (documentation and GitHub repository only).

- Create the repository and honest docs.
- Reserve `App/` and `Tests/` folders without fake implementations.
- Do not write audio, networking, security, or UI code.

## Parked (needs a Mac and Xcode later)

### Milestone 2 — Audio input/output proof of concept

Local capture and playback on one iPhone. No network. Prove that the app can record from the current input route (hopefully AirPods mic) and play to the current output route (hopefully AirPods).

Start this only after there is a Mac. Do not begin it on Windows.

### Milestone 3 — Nearby iPhone discovery

Browse and advertise Bubble devices with Network.framework and Bonjour. No audio. Prove two phones can see each other nearby.

### Milestone 4 — Basic device-to-device data transfer

Open a bidirectional `NWConnection` and send a small non-audio payload both ways. Prove the transport works before attaching audio.

### Milestone 5 — Transmit test audio data

Send a known buffer or file of audio samples (not live mic) over the connection and play it on the other phone. Prove framing, format, and playback clock independently of live capture.

### Milestone 6 — Live one-way voice

Mic on phone A → transport → speaker/AirPods on phone B. One direction only.

### Milestone 7 — Live two-way voice

Both directions at once. This is the first time the intended Bubble path exists as software. It is still unproven for AirPods, ANC, and latency until Milestone 10.

### Milestone 8 — Voice processing and latency optimization

Documented voice-processing paths, format/codec choices, and measured latency. Optimize only after there are numbers from real devices.

### Milestone 9 — Security / pairing

Authenticate the peer before audio flows. Prefer documented TLS and pairing patterns on Network.framework. Do not ship an open audio pipe.

### Milestone 10 — Real-world AirPods testing and performance

Two iPhones, two sets of AirPods, noisy environment, ANC on if the system allows it, battery and thermal notes. Until this passes, do not say the product concept works.

## Out of scope for the prototype

- Recreating Apple Audio Sharing or any private AirPods protocol
- Long-distance calling (this is nearby device-to-device)
- Assuming ANC, mic quality, or Bluetooth HFP routing can be controlled if no public API does that
- Implementing all modules in one change
- Creating an Xcode project or Swift sources while there is no Mac
