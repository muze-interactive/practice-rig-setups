# Rigs

## Objectives

The rigs outlined here are specific to the bass instrument,
although concepts can be used for others, such as guitar,
keyboards, etc.

### Practice

Practice regimen requires:
- The ability to play and hear clearly what I'm playing
- Play with/against some form of backing track (or metronome)

Ability to record is useful:
- Share with others (like an instructor)
- Self-review

### Performance

The idea of performance is to play well and sound good for others,
work with venues' sound engineers and their platforms.

### Content Creation and Music Production

The idea of content creation includes ability to create instrument tracks
as part of song construction.
Choice of instrument and specific gear choices can help craft a specific
tone demanded by a song.

Skills and knowledge required include sound engineering and music production.
Building skills requires iterative experimentation with gear,
but based on technical proficiency on the instrument.

Gear can get expensive though (GAS),
you'll see gear selection strategies later.

## Rig Framework

First is it useful to describe what components can sit in a rig,
and where each might sit:

1.  Instrument
2.  FX
    -   Dynamics
    -   Distortion/Overdrive
    -   Modulation
    -   EQ/Filtering
    -   Time/Spatial
    -   Pitch/Harmony
3.  Preamp
    - Amp
        - IR/Cab
4.  DI -> Used to feed Front-of-House PA.
5.  ADDAC (Device + Software Driver)
    -   Inputs from previous signal chain - Instrument can bypass and go direct here.
        - Between 2 to 4 input channels
    -   Outputs
        - Between 2 to 4 output channels
        - Audio output to Studio Monitors and/or Headphones
    -   Workstation Software Driver Router configurations (see next sections for routing)

6.  Workstation setup
    -   ADDAC SW Routing
        -   Separate inputs (2 to 4), configured inputs for virtual router
        -   Outputs 1+2 Stereo, route to studio monitors and headphones
        -   Ouptuts past 1&2 not used
    -   Virtual Router (Loopback on Mac)
        -   Output channels 1+2 as standard stereo output that will have separate monitor
        -   Backing track playback sources (see next section for Backing Track sources) routed as Outputs 1 & 2 (stereo).
        -   Instrument inputs from ADDAC routed to outputs 3+.
            If instrument inputs are configured as stereo from ADDAC SW inputs, the associated router outputs would be paired to outputs 3+4, 5+6, etc
        -   If instrument inputs are mono from the ADDAC SW inputs and desired to render on a stereo output, split the inputs to the associated output pair 3+4, 5+6, etc.
        -   Passthrough channels route stereo from 1+2 to Output channel 1+2.
        -   Monitor output from the Output channel pairs (1+2, 3+4, 5+6) to ADDC SW output channels 1+2 for audio
    -   Backing Track sources
        - Live
            - Streaming via Browser, Gaming or other streaming specific software
            - Configured as SW router sources
        - Pre-recorded
            - Music/Video players
            - DAW
            - Browser or app-based

    The idea here is to simplify routing for configuration of software processing components, such as DAWS and Video Streaming/Recording, which we cover next.

7.  Signal processors (running on workstation)
    - Recording software
        - Simple live recording
    - Digital Audio Workstation (DAW, Logic Pro on Mac)
        - In addition to Recording SW, ability to edit, mix and master tracks.
        - Pre-recordning backing tracks or stems can be imported directly as audio tracks, without consideration of signal chain.
        - Backing tracks can be recorded live - via audio track monitoring input channels 1+2
        - Instrument tracks can be recorded live - via audio track monitoring input channels 3+
        - DAW monitor mode *should* work OOB with the standard monitor setup in the SW router without explicit configuration.
        - DAW plugins (VSTs) can be used to add FX, Preamp, Amp and IR profiles inside the DAW.  This has implications of what comprises the signal chain.
    - OBS video capture, streaming, recording
        - Input sources
            - Display screen
            - Web Cams (multiple supported)
            - Audio can use any of the following:
                - Router output channels (show up as system device)
                - SW monitor configuration
                - Display audio capture
                - External microphone (via system device config)
        - Scene configuration sets up video and audio input source and layout configurations

## Signal Chains

Note that the signal chain configuration can vary in how the framework is configured and used.

All of the components listed could be considered part of the signal chain depending on context.

The following are the different configurations I run.

### Home Studio Setup Rig

The home studio rig is optimized for practice and recording dry (no upstream FX).
It can be used strictly for practice,
or bass track recording where FX are added to the dry track in the DAW.

1.  Instrument is typically EHB style bass with both options for active/passive pickup/EQ configs.

2 -> 4 typically not used for practice (instrument direct to ADDC, dry).
    Line6 HX Stomp inserted when experimenting / crafting tones for
    sound engineering and/or music production (although FX can be handled at the DAW, see below).

5.  A Focusrite Scarlet 4i4 Gen3
    - Outputs to Adam 5 studio monitors, Audio Technica wired headphones.

6.  A Mac Mini hosts the workstation
    - Loopback is used as the Router

7.  Audio capture:
    - Quicktime (simplest recording for quick share)
    - Logic Pro (DAW, recording for backing tracks) - FX can be added to the signal chain downstream of the dry audio track via VST plugins.
    - OBS for audio+video performance recording

### Travel Practice/Jam Setup Rig

This differs from the Home Studio setup in that it is general purpose,
and not necessarily optimized for any of the 3 objectives.

1.  Instrument is typically EHB style bass with both options for active/passive pickup/EQ configs.

2 -> 5  Line6 HX Stomp functions as FX provider, rudimentary pre-amp and ADDAC via USB.
    The tradeoff is a small amount of latency via USB connection.
    It does have a headphone out jack, which is a way to remove latency during practice/performance.

6.  A Macbook air hosts the workstation
    - Loopback is used as the Router

7.  Audio capture:
    - Quicktime (simplest recording for quick share)
    - Logic Pro (DAW, recording for backing tracks) - FX can be added to the signal chain downstream of the dry audio track via VST plugins.
    - OBS for audio+video performance recording

While this setup is flexible and minimal,
it has tradeoffs:

- Quality in audio output (space vs quality of wired headphones)
- Latency disparity
- Limited CPU power can lead to reduced audio streaming/sampling quality
- Limited screen real estate space.

### Performance Rig

The idea is to sound good for gig performances,
and have some flexibility to work with venue sound systems and constraints:

1.  Instruments typically include a couple of traditional style basses with onboard active pickups, different tonal options and EQ.
    This allows more flexibility of controlling tone/EQ at the bass, although rarely needed.

2.  Line6 HX Stomp functions as FX provider, or simple compressor (MX mini series)

3 -> 4.  Trace Elliot ELF funcions as both pre-amp, monitor amp,
        With DI out for FOH, and TE 10" cab.
        For outdoor venues, the TE may be a little weak,
        where GK Fusion 112 combo can be chained from TE DI out to return or input,
        and function as extended monitor.  
        The TE 10" cab coupled with GK 12" combo cab sounds great together
        for clarity as a monitor combo, albeit unorthodox.
        The DI from GK pre-amp can be used in pre-mode to drop GK pre-amp
        for FOH for a more transparent sound, of FOH engineer prefers.

    The remainder of the signal chain is not currently used for live gigs,
    although may design out more formal venue band capture for live band recordings, with extended mixing and DAW input capture.

## Future directions

There are a few alternate directions under research that may
alter my current rig setups listed above:

1.  Replacement of Line6 HX Stomp with Neural DSP Quad Coretex,
    or Darkglass Anagram.

2.  Work station transition to Linux platform from Mac OS.

3.  DAW Platfrom switch to Reaper from Logic Pro.

The two primary reasons for the above:

-   Mac platform evolution obsolescence for supported ADDC and other sound gear:
    -   Later 2026, Apple will drop Rosetta Intel emulation support.
    -   It is questionable whether Apogee or Focusrite will supply drivers for
    Apple Silicon platform.
    -   Logic Pro is supported only on Mac.
    -   Reaper works well on SteamOS and Linux platforms - may have interesting
        options for live recording.
-   Simplification of Live performance rigs 
    -   Modern software FX, pre-amp/amp/IR modelers can fit in one device (HX stomp already does this)
    -   Simplify workflow, presets
    -   Integrated CI and ground lift (HX stomp does not do this).
    -   In short, HX Stomp and DI box replaced by single device - 
        The current DI solution with ELF/GK fusion is a hack,
        particular if travelling for gigs/jams, and cannot bring monitor setup.

