# AirPods Bubble

AirPods Bubble is a proof-of-concept for Charlie Bishop. It asks whether two nearby iPhones can exchange live voice so that each person’s AirPods microphone is heard in the other person’s AirPods, using only documented public Apple APIs.

This is not a recreation of Apple’s proprietary AirPods software, Audio Sharing, or any private system feature. The full two-way pipeline has **not** been built or proven on devices.

## Current status

**Milestone 1 — project setup.** The repository and planning documents exist. There is no runnable app, no audio pipeline, and no networking implementation.

| Item | Status |
|---|---|
| Product concept | Written (`Concept/`) |
| Planning docs | Written |
| iOS app | Not started |
| Audio input/output | Not started |
| Nearby discovery / transport | Not started |
| Pairing / security | Not started |
| Real iPhone + AirPods tests | Not started |

## How to run it

You cannot run AirPods Bubble yet. There is no Xcode project and no build.

Later milestones will need:

- A Mac with a current Xcode
- Two iPhones
- Two pairs of AirPods (or one pair per phone under test)
- This repository cloned onto the Mac

On this Windows machine the repo is documentation only.

When an app exists, this section will name the exact Xcode scheme, bundle identifier, and run steps. Until then, treat any claim that Bubble “works” as unproven.

## How it is built

There is no toolchain build yet. The intended stack, to be confirmed when implementation starts:

- Swift, SwiftUI, and an Xcode iOS app
- Audio: `AVAudioSession`, `AVAudioEngine`, `AVAudioInputNode`, `AVAudioPlayerNode`, documented converters/codecs
- Networking: Network.framework (`NWBrowser`, `NWListener`, `NWConnection`), Bonjour, peer-to-peer options that Apple documents
- Not the basis of the design: Multipeer Connectivity, private Apple APIs, or invented APIs

Intended modules (folders exist; they are empty on purpose):

```text
App/
├── Audio/
├── Networking/
├── Security/
├── UI/
└── Utilities/
Tests/
```

Build and test commands will be added when they exist. Do not assume a `xcodebuild` invocation until Milestone 2 creates the project on a Mac.

Version **0.1.0** is documentation and repository setup only.

## Documents

- [PROJECT_PLAN.md](PROJECT_PLAN.md) — milestones and what is in scope now
- [RESEARCH.md](RESEARCH.md) — APIs and topics already researched; documented vs assumed vs untested
- [ARCHITECTURE.md](ARCHITECTURE.md) — proposed modules and pipelines (not implemented)
- [TESTING.md](TESTING.md) — how each milestone will be tested on real devices
- [CHANGELOG.md](CHANGELOG.md) — notable changes
- [Concept/](Concept/) — original product concept

## Rules for this project

1. Use documented public Apple APIs only.
2. Do not invent APIs or use private Apple APIs.
3. Do not build the new implementation around Multipeer Connectivity.
4. Label documented behavior, assumptions, and experimental results separately.
5. Do not treat the complete Bubble pipeline as working until it is tested on real iPhones and AirPods.
6. Keep modules independently testable. Do not implement everything at once.
