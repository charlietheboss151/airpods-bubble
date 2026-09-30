# Research

This file records what has been studied and how confident we are. **Nothing here is an implementation claim.** If a behavior is not cited as documented Apple API behavior and has not been measured on device, it is an assumption or an open question.

Status of this file: topic inventory from prior research. Detailed citations and device results will be added when we implement and test.

## How to read the labels

| Label | Meaning |
|---|---|
| **Documented** | Public Apple API or documented system behavior. Still must be verified for our use case. |
| **Assumption** | Reasonable, not proven for Bubble. Must not be treated as fact in code comments or UI. |
| **Experimental** | Must be tested on real iPhones and AirPods. Outcome unknown. |
| **Out of scope / do not use** | We will not build on this. |

## Audio session and engine

| Topic | Label | Notes |
|---|---|---|
| `AVAudioSession` | Documented | Category, mode, and options control how the app shares audio with the system. Exact category/mode for Bubble is not chosen yet. |
| `AVAudioEngine` | Documented | Graph of input, processing, and output nodes. |
| `AVAudioInputNode` | Documented | Captures from the current input route. Does not guarantee AirPods mic. |
| `AVAudioPlayerNode` | Documented | Schedules buffers for playback on the current output route. |
| Voice processing (session / engine) | Documented + experimental | Apple documents voice-processing APIs. Effect on AirPods, ANC, and latency is untested for Bubble. |
| `bluetoothHighQualityRecording` | Documented + experimental | Documented session option. Whether it applies and helps with AirPods is untested here. |
| Bluetooth HFP vs other AirPods routes | Assumption + experimental | Hands-Free Profile often implies narrow-band or hands-free mic quality. We have not measured Bubble’s route. |
| AirPods audio routing | Experimental | The system, not this app, picks the route. We can request session options; we cannot assume AirPods stay selected. |
| Active Noise Cancellation | Assumption | No public API identified in this pass that lets an app turn ANC on or keep it on. “ANC can stay enabled” is a product hope, not a proven control. |
| Audio mixing with the user’s music | Experimental | Mixing voice over other audio depends on session category and system policy. Untested. |
| Audio Sharing (system feature) | Out of scope / do not use | Apple system feature. We are not reimplementing it. |

## Encoding and formats

| Topic | Label | Notes |
|---|---|---|
| PCM between engine nodes | Documented | Typical local format inside `AVAudioEngine`. |
| `AVAudioConverter` | Documented | Sample-rate and format conversion. |
| Audio Converter Services | Documented | Lower-level conversion. Use only if the AVAudio path is insufficient. |
| Audio Codec Services | Documented | Compressed codecs exist. Codec choice for live voice is not decided. Do not invent a codec API. |
| Latency vs compression | Assumption | Less compression / smaller frames may reduce latency and increase bandwidth. Measure in Milestone 8. |

## Networking

| Topic | Label | Notes |
|---|---|---|
| Network.framework | Documented | Chosen transport family. |
| `NWBrowser` | Documented | Discover services. |
| `NWListener` | Documented | Accept connections. |
| `NWConnection` | Documented | Bidirectional byte/datagram streams. |
| Bonjour | Documented | Service advertisement/browse. Service type name is not finalized. |
| Peer-to-peer on Network.framework | Documented + experimental | Apple documents peer-to-peer parameters. Range, Wi-Fi/Bluetooth fallback, and airplane-mode behavior must be tested. |
| QUIC / TCP / UDP | Documented | Transport to pick later. QUIC and TCP have TLS stories; UDP needs an explicit reliability/security plan. |
| TLS on Network.framework | Documented | Pairing/identity design is Milestone 9. Unauthenticated audio is not the end state. |
| Multipeer Connectivity | Out of scope / do not use | Do not build the new implementation around it. |
| Nearby Interaction | Documented + not the transport | Distance/direction (U1 where available). Not a voice pipe. May be researched later for “are we nearby?” It is not Milestone 3’s transport. |

## Device, battery, and product constraints

| Topic | Label | Notes |
|---|---|---|
| Two-phone requirement | Documented constraint | Audio goes phone → nearby phone → AirPods. There is no public path that sends one AirPods mic straight into another pair without phones. |
| Battery / thermal | Experimental | Continuous capture, encode, send, decode, and play will cost energy. Measure in Milestone 10. |
| Privacy / microphone permission | Documented | The app must request mic access. Networking local-network permission applies on modern iOS. |
| Airplane / noisy venues | Experimental | The product story (planes, trains) may restrict radios. Untested. |

## Open questions (do not answer in code by guessing)

1. Can we get a usable AirPods microphone route while output stays on AirPods, at a quality that sounds like conversation rather than a phone call?
2. Does ANC stay on while we capture and play, and can we tell from public APIs?
3. Can the user keep their own media playing under or beside incoming voice?
4. What nearby radio path works in the environments we care about (including constrained Wi-Fi)?
5. What latency is acceptable, and what do we actually measure?
6. How do we pair two phones so a stranger cannot join the bubble?

Until those are tested, architecture diagrams are proposals.
