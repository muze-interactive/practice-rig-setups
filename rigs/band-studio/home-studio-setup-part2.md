# Home Studio Setup Part 2

I’m a multi-instrumentalist (bass, keyboard, guitars).  I have a small home studio based on mac os with Logic Pro and/or Ableton Live, and a scarlett focusrite gen 3 (see the README and associated [rigs](./rigs) directory for details).

## Objectives

I’d like to expand out my home studio to accommodate up to a five piece band setup (2 vocals, guitar, bass, keyboards, drums (up to 6 mic channels), and have small group performance either at home, or a small indoor venue without FOH, or exclude PA and plugin into a existing FOH system at another venue.

For home/indoor studio, I'm thinking the PA active speakers function as both live monitors and PA, although I'd like capability to do IEMs for practicing for live venues.
For large outdoor venues, I should be able to tie mixer directly to FOH PA active speakers, but I can reuse my active speakers as stage monitors, or, my mixer ties directly to IEM system.

## Requirements

### Mixing / Line Level adjustment

- Mixing for following input signal chains (up to 16 channels):
  - Bass
  - Guitar
  - Keyboards
  - Vocals (2)
  - Drums (up to 6 channels)
  - Talkback / comms cue mics (up to 4) — see Comms / Talkback below
  - Backing Track (Stereo via Bluetooth, USB, dual XLR and/or TRS stereo) - this could also introduce Ableton into playback chain (or not)
- Assume mono inputs by default (except backing track),
  although band practice/performance with fewer members may use stereo inputs within limits of the mixer
- Tablet/smart phone/remote control
- Outputs of mixer
  - FOH PA Stereo
  - 2 Monitors (self powered, or feed to separate amp for monitors)
  - USB to a DAW for recording
  - Control via iPad

### Monitors

I need the flexibility to do stage monitors (up to 3), or, IEMs.

- My PA active speakers I can use as floor (wedge) monitors.
- I can also use IEMs, so will need a system to support that

### Wireless

I'm not sure about IEMs wired/vs wireless.  If I need IEM wireless, I figure I should explore wireless for mobile instruments, like guitar, bass and vocals

### PA

- Prefer PA cabs with internal amplifiers (so I don't need separate rig for PA amps)
- Amplification for a basement or large living room, or perhaps coffee-shop size venue.
- Battery powered is a plus.

### Other

- Portable (can load up in an SUV or small truck)

> **Selected build:** the home studio centers on the **Allen & Heath CQ-18T** —
> documented as a standalone rig in
> [`rigs/band-studio/home-studio.md`](./rigs/band-studio/home-studio.md). The
> value-tier gear list below builds around it.

## Candidate Gear (Value Tier, ~$2.3–3.3k all-in)

The keystone of this build is a **digital mixer that doubles as a multitrack USB
interface with tablet/phone app control**. One box then covers mixing, remote
control, per-channel passthrough to the Mac DAW, and both backing-track paths
(native Bluetooth *and* USB return from Ableton/Logic). Powered PA tops, an
optional sub, and monitors hang off it.

### Channel count target

Worst-case simultaneous inputs from the requirements:

| Source | Channels |
|--------|----------|
| Bass | 1 |
| Guitar | 1 |
| Keys | 1–2 |
| Vocals | 2 |
| Drums (up to 3) | 3 |
| Backing track (stereo) | 2 |
| **Total** | **~11–12** |

A 16-channel-class mixer gives comfortable headroom (extra drum mic or stereo
keys). 12-channel is workable but tight on mic preamps once 3 drum mics are in.

### Mixer — core of the rig

The core is the **Allen & Heath CQ-18T (~$1,499)**. One box covers mixing, remote
control, per-channel USB passthrough to the Mac DAW, both backing-track paths, and
aux sends to wedges and IEMs.

| Item | Model | Approx. Price | Why |
|------|-------|---------------|-----|
| Digital mixer | **Allen & Heath CQ-18T** | ~$1,499 | 16 mic pres (8 XLR + 8 combo with Hi-Z) + dedicated stereo line in, 24×22 USB multitrack to the Mac (per-channel DAW passthrough), **built-in touchscreen** + iPad/phone apps (CQ MixPad / CQ4You), **native Bluetooth**, 6 aux "Mix" outs (stereo-linkable) + Main L/R, and an **officially documented MIDI control protocol**. |

The built-in touchscreen means the mixer stays usable even if the apps lapse, and
native Bluetooth covers the wireless backing-track path with no dongle. USB return
from Logic/Ableton covers the **USB path**.

#### Control & automation

- **Mackie Control (MCU/MCP)** is a MIDI *control-surface* protocol for driving a
  **DAW** (Logic, Ableton). Fully documented. Relevant to DAW voice-automation,
  **not** to controlling the mixer.
- **The CQ MIDI protocol** is how third-party code controls the CQ-18T itself.

A&H **publishes the mixer control protocol spec**:

- **Officially documented MIDI protocol** — a downloadable PDF (*CQ MIDI Protocol
  V1.2* on the CQ-18T resources page). Vendor-supported, so you code against a real
  spec rather than reverse-engineering.
- **Transport:** MIDI over **USB or network** (MIDI-over-TCP), MIDI channel 1. Drive
  it from Python (`mido` / `python-rtmidi`) — no Wireshark needed.
- **Covered:** channel **levels/faders** (NRPN), **mutes**, **sends**, and **scene
  recall** (Program Change) — i.e. the whole "light mixer adjustment via voice" set.
- **Gap:** deeper functions (e.g. multitrack-record transport) use a *separate,
  undocumented TCP protocol* that CQ MixPad speaks — reverse-engineering territory,
  but the documented MIDI covers mixing control.
- **No native OSC** (A&H's position: use their MIDI protocol). Third-party proof it
  works: a **Bitfocus Companion** module exists (`allenheath-cq`).

**Voice-automation notes** (see the separate [muzicbox](https://github.com/muze-interactive/muzicbox)
project — Python):

- *DAW control* is independent of the mixer: Logic and Ableton both speak **Mackie
  Control (MCU)** over MIDI, and Ableton also does OSC (AbletonOSC / Max-for-Live).
  `voice → intent → MIDI(MCU)/OSC → DAW` is well-trodden.
- *"Light mixer adjustment via voice"* (mute channel, bump a monitor, raise vocals)
  maps to **mute / fader / aux-send** — all covered by the documented CQ MIDI
  protocol, so muzicbox can code against the official spec.

Sources: [Mackie Control protocol notes](https://github.com/NicoG60/TouchMCU/blob/main/doc/mackie_control_protocol.md),
[CQ-18T resources (MIDI protocol)](https://www.allen-heath.com/hardware/cq/cq-18t/resources/),
[Bitfocus Companion — A&H CQ](https://bitfocus.io/connections/allenheath-cq).

### PA — powered tops (buy a pair)

For basement / large living room / coffee-shop, a pair of 12" powered tops:

| Item | Model | Approx. Price (pair) | Notes |
|------|-------|----------------------|-------|
| PA tops | **Mackie Thump212** | ~$600 | Budget king; app + Bluetooth, angled cabinet **doubles as a floor wedge** — flexible for the value build. |
| PA tops (step-up) | **Mackie SRM212 V-Class** | ~$1,200 | Better DSP/sound and build than Thump if the budget flexes; same wedge-capable cabinet. |

### Monitors (2)

| Item | Model | Approx. Price | Notes |
|------|-------|---------------|-------|
| Wedges | Reuse Mackie Thump212 tilted as wedges, **or** add 2× Mackie Thump212 | $0–$600 | Driven by the mixer's aux sends. Start with tops-as-wedges to hold budget, add dedicated wedges later. |

### In-Ear Monitors (IEMs) — add-on

The mixer is the IEM engine — each performer's monitor mix is an **aux send**.
The CQ-18T's 6 aux Mixes (stereo-linkable) give up to 6 mono or 3 stereo IEM
mixes, plenty for a 5-piece even with a couple of wedges still running. With
**CQ MixPad / CQ4You, each musician can control their own aux mix from their own
phone or tablet**, which replaces dedicated personal-monitor-mixer hardware.

For the wireless ears themselves, standardize on the **Shure PSM300**:

| System | Price/set | Band | Notes |
|---|---|---|---|
| Shure PSM300 | ~$829/set | UHF | Reliable pro standard; UHF stays **off the 2.4 GHz Wi-Fi band**, so no contention with mixer control (wired *or* Wi-Fi). Add one set per performer who needs to move. |

**Budget wired alternative for stationary players (drummer, keys, bass):** run a
wired belt-pack headphone amp off an aux send — zero latency, no RF, no batteries.
Cheaper than a PSM300 set per person, and by itself is enough for home practice.

**Earphones:** the PSM300 system bundle (P3TR112GR) ships with **Shure SE215**
earphones — the real sound-quality lever, so no separate earbud budget for those
sets. Players on a wired belt-pack still want their own SE215 (~$100) or budget
KZ/Moondrop ($20–50).

**Recommended path — add-on phase:** one **Shure PSM300** set per performer who
moves (2 vocals, guitar) + wired belt-packs for stationary players
(drummer, keys, bass), adding PSM300 sets as budget allows. **IEM add-on: roughly
$1,700 (2 PSM300 sets) to ~$2,500 (3 sets)**, less if more players stay wired.

**Comms / talkback over IEMs:** once players are on ears they lose the ambient
"talk to each other" path, so add dedicated cue mic(s) routed to every monitor mix
(off the Mains). On the CQ-18T this costs 1–4 mic pres and no new IEM hardware —
full detail in the [home studio doc](./rigs/band-studio/home-studio.md).

### Microphones & stands

Bass, guitar, and keys go in via DI/line, so mics are mainly for the 2 vocals
and up to 3 drum channels. An SM57 doubles as a snare *and* guitar-amp mic if you
ever mic a cab instead of DI.

| Item | Model | Approx. Price | Notes |
|------|-------|---------------|-------|
| Vocal mics (2) | Shure SM58 | ~$99 ea (~$200 pair) | Industry-standard dynamic; budget option: Behringer XM8500 (~$25 ea). |
| Kick mic | Shure PGA52 or AKG D112 | ~$99 / ~$199 | Dedicated low-frequency dynamic for kick drum. |
| Snare / utility mic | Shure SM57 | ~$99 | Snare, and doubles as a guitar-amp mic. |
| Overhead | 3rd drum channel: single pencil condenser (Behringer C-2 pair ~$60, or MXL 990) | ~$30–70 | Captures cymbals/kit ambience for the 3rd drum channel. |
| Drum kit alt. | Shure PGA Drum Kit 4/6 or Audix FP kit | ~$250–400 | Bundle covering kick/snare/overheads in one box. |

| Item | Model | Approx. Price | Notes |
|------|-------|---------------|-------|
| Vocal boom stands (2) | On-Stage MS7701B or similar | ~$30 ea | Tripod boom for standing vocalists. |
| Drum mic stands (2–3) | Short boom + kick tripod | ~$25–40 ea | Low-profile booms for snare/overhead; tripod for kick. |
| Speaker stands (pair) | On-Stage / Ultimate Support | ~$60–100 pair | For the powered PA tops. |
| Mic clips & cables | XLR cables + clips as needed | ~$15–25 ea | One XLR run per mic back to the mixer. |

Rough add for a full mic locker + stands at the value tier: **~$500–700**, so plan
mics/stands as a phase-2 add on top of the ~$2.3–3.3k mixer + PA budget, or fold a
minimal set (2× SM58 + kick + SM57) into the balanced bundle.

### Optional / add-later

| Item | Model | Approx. Price | Notes |
|------|-------|---------------|-------|
| Sub | Mackie Thump118S / QSC KS112 / RCF Sub 705-AS II | $600–900 | Adds low end for bass + kick in a real room; skip for living-room-only. |
| DI boxes | Radial / Behringer DI | $30–100 ea | For keys and stereo backing-track feeds. |
| Stands, cables, distro | Speaker stands, XLR runs, power strip, rack/gig bag | ~$300 | Consumables and transport. |

### Example value bundles

- **Tightest (~$2.3k):** CQ-18T + 2× Mackie Thump212 (tops, reused as wedges) +
  cables/DI/stands. No dedicated sub, monitors, or IEMs yet.
- **Balanced (~$3.3k):** CQ-18T + 2× Mackie SRM212 (tops) + 2× Thump212 dedicated
  wedges + cables/DI/stands. Add a sub and PSM300 IEMs when the
  room and budget call for it.

### How this satisfies the requirements

- **Mixing / line level:** 16 mic pres cover all listed sources with headroom.
- **Both backing-track paths:** native Bluetooth + USB return (Ableton/Logic), plus
  the dedicated stereo line input.
- **Tablet/phone control:** CQ MixPad / CQ4You over Wi-Fi or Ethernet, plus the
  built-in touchscreen.
- **Per-channel DAW passthrough:** CQ-18T as a 24×22 USB interface into Logic/Ableton.
- **Outputs:** stereo main → PA tops; aux sends → 2 monitors.
- **Powered PA:** all speakers listed are self-powered (no separate amp rig).
- **Portable:** compact mixer + two tops load easily into an SUV.
