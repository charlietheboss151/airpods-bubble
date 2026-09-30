# Testing

Tests follow the milestones in [PROJECT_PLAN.md](PROJECT_PLAN.md). A module test that passes on a simulator is not a substitute for AirPods hardware tests.

There are **no automated tests in this repository yet**. Do not add tests that mock the full Bubble pipeline and call it proven.

Device and Xcode tests are parked until there is a Mac. Do not add a simulator target or CI iOS job as a stand-in.

## Principles

- Test the smallest piece that can fail.
- Record device model, iOS version, AirPods model, and session route (input/output) in the result.
- Separate **documented API used**, **what we assumed**, and **what we measured**.
- Simulator results cannot close Milestones 2–10 for AirPods behavior.

## Milestone success tests

| Milestone | Success means | Where |
|---|---|---|
| 1 | Repo and docs exist; no fake “working” app | This repository |
| 2 | One iPhone captures and plays locally; route logged | Physical iPhone; AirPods if available |
| 3 | Two iPhones discover each other by the Bubble service | Two physical iPhones, nearby |
| 4 | A known non-audio message arrives both ways | Same |
| 5 | A known audio buffer sent from A plays on B | Same, plus hearing the clip |
| 6 | Live speech on A is heard on B, one way | Same, plus AirPods on B at least |
| 7 | Live speech both ways | Two phones, ideally two AirPods pairs |
| 8 | Latency and quality numbers recorded; changes justified by those numbers | Same |
| 9 | Unpaired peer cannot receive audio; paired peer can | Same |
| 10 | Same as 7 in a noisy environment, with battery notes; ANC/media mixing reported as measured, not hoped | Real-world venue |

## What to log on device tests

- Date and testers
- iPhone A/B model and iOS
- AirPods A/B model, and whether they stayed the active route
- `AVAudioSession` category, mode, options
- Reported input/output port names
- Network path (Wi-Fi, peer-to-peer, etc.) if the system exposes it
- One-way and round-trip latency if we can measure it
- Failures and surprises (route flips, HFP, ducking, ANC off, permission prompts)

## Unit and integration tests (later)

When code exists:

- **Audio:** format conversion and framing without a device, plus a manual device checklist for Milestone 2.
- **Networking:** framing and message IDs without radios if we can; discovery and connection only on devices.
- **Security:** reject unauthenticated peers in tests that do not require AirPods.

Do not add a green CI badge that implies iOS device tests ran.
