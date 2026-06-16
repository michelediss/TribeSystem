# FX Engine

Status: v0.1 closed

This document defines the TribeBox internal FX engine.

TribeBox FX are designed for live Tekno, Tribe, Hardtek and Acid performance. The goal is not to replicate a DAW-style insert chain, but to provide a compact, predictable and highly performative effect system.

## Core Principles

- Effects must be playable with physical controls.
- Routing must remain predictable during live performance.
- The engine must favour immediacy over deep modular routing.
- Morph Scenes must be able to modulate all major FX parameters.
- Delay and reverb are global send effects, not per-track processors.
- The output stage must include safety protection for live use.

## Global Architecture

```text
Track 1 Kick    -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
Track 2 Bass    -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
Track 3 Perc 1  -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
Track 4 Perc 2  -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
Track 5 Hat     -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
Track 6 Perc 3  -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
Track 7 Acid    -> Acid Drive -> Acid Filter -> Amp -> Delay Send -> Reverb Send
Track 8 Sampler -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send

All tracks -> Mix Bus -> Performance FX -> Safety Limiter -> Output
```

The routing is fixed and performance-first. It should not behave like a freely patchable DAW mixer. Instead, the routing should remain stable while Morph Scenes and macros control the important parameters.

## Per-Track FX

Every track has the same basic FX structure:

```text
Drive -> Filter -> Amp -> Delay Send -> Reverb Send
```

Per-track parameters:

- drive type
- drive amount
- filter type
- filter cutoff
- filter resonance
- filter envelope amount, where supported by the engine
- delay send amount
- reverb send amount

Track 7 Acid can use Acid-specific defaults for drive and filtering.

## Filter Types

The v0.1 filter set is:

```text
LP12
LP24
HP12
BP12
Notch
Peak / Resonator
Acid LP
```

### Filter Roles

| Filter | Main use |
| --- | --- |
| `LP12` | Smooth filtering for samples, hats and percussion |
| `LP24` | More aggressive low-pass filtering for kick, bass and acid |
| `HP12` | Build-ups, breakdowns and low-end removal |
| `BP12` | Percussion, acid movement and rhythmic FX |
| `Notch` | Psychedelic sweeps and live movement |
| `Peak / Resonator` | Metallic percussion, noise and resonant texture |
| `Acid LP` | Dedicated Acid Synth filter character |

The common filters should be available to all tracks. `Acid LP` is primarily intended for Track 7 Acid, but may be exposed to other tracks later if useful.

## Drive

Drive is selectable per track, with different defaults per track.

v0.1 drive types:

```text
Soft Clip
Hard Clip
Asymmetric Clip
Acid Overdrive
Digital Crush
```

### Default Drive Types

| Track | Default drive |
| --- | --- |
| Track 1 Kick | Hard Clip |
| Track 2 Bass | Soft Clip |
| Track 3 Perc 1 | Hard Clip |
| Track 4 Perc 2 | Hard Clip |
| Track 5 Hat / Ride | Light Digital Crush |
| Track 6 Perc 3 / Noise / Utility | Digital Crush |
| Track 7 Acid Synth | Acid Overdrive |
| Track 8 Sampler | Soft Clip |

### Drive Position

v0.1 uses a simple fixed position:

```text
Drive -> Filter
```

This means the drive stage happens before the filter. This keeps the signal flow simple, predictable and useful for aggressive tekno/acid sound design.

Future versions may consider pre/post drive routing, but v0.1 should stay fixed.

## Delay Architecture

TribeBox has one global send delay.

Each track has:

```text
Delay Send Amount
```

The global delay is shared across the whole project and should be designed as a performative dub/acid delay rather than a generic studio effect.

### Delay v0.1

```text
Stereo Sync Delay
Ping Pong Mode
Feedback
Delay Filter
Delay Drive
Freeze
```

### Delay Parameters

- time / sync division
- feedback
- filter cutoff
- filter tone
- stereo width
- ping pong amount
- drive
- freeze
- duck amount

### Delay Performance Actions

The delay must support live gestures:

- Delay Throw
- Delay Freeze
- Feedback Rise
- Dub Echo
- Tail Cut

Delay Throw should temporarily increase the delay send of the selected track or target group without permanently changing the track's stored send amount.

Delay Freeze should hold the delay buffer in a controlled way suitable for live performance.

## Reverb Architecture

TribeBox has one global send reverb.

Each track has:

```text
Reverb Send Amount
```

The reverb is a performance reverb, not a realistic acoustic simulation. It should be useful for breakdowns, swells, dark washes and live transitions.

### Reverb v0.1

```text
Plate / Hall Hybrid
Dark Mode
Low Cut
High Cut
Decay
Freeze / Infinite
```

### Reverb Parameters

- size
- decay
- pre-delay
- tone
- low cut
- high cut
- width
- freeze

### Reverb Performance Actions

The reverb must support live gestures:

- Reverb Swell
- Reverb Freeze
- Tail Cut
- Dark Wash

Reverb Swell should temporarily increase reverb send and/or decay for the selected track, a target group or the full mix depending on the current performance target.

## Performance FX

The required v0.1 Performance FX are:

```text
Master Filter
Stutter / Beat Repeat
Delay Throw
Delay Freeze
Reverb Swell
Safety Limiter
```

### Master Filter

A master performance filter placed on the performance bus.

Required uses:

- high-pass build-ups
- low-pass breakdowns
- global sweeps
- Morph Scene transitions

The Master Filter should be controllable by Morph Scenes and performance macros.

### Stutter / Beat Repeat

A rhythmic performance effect for fills, glitches and transitions.

Required controls:

- enable / amount
- repeat rate
- gate length
- mix
- reset / release

The effect should be safe for live use and must return cleanly to the dry signal.

### Delay Throw

A performance gesture that sends selected material into the global delay.

Possible targets:

- selected track
- active track group
- full mix, if explicitly selected

It must not permanently overwrite the stored track delay send unless explicitly saved.

### Delay Freeze

A performance gesture that freezes the delay buffer.

Delay Freeze should be available in FX Mode and as a Morph Scene target.

### Reverb Swell

A performance gesture for transition washes and breakdowns.

It may control:

- selected track reverb send
- reverb decay
- reverb mix / return level
- reverb tone

### Safety Limiter

A final-stage limiter is mandatory.

Purpose:

- protect the live output
- avoid extreme peaks during feedback, drive and Morph Scene transitions
- act as safety, not as a creative compressor

The limiter should be transparent by default and always-on unless disabled in a protected system setting.

## Performance Routing

The routing is fixed but performance-oriented:

```text
Tracks -> Per-track Drive/Filter -> Sends -> Mix Bus -> Performance FX -> Safety Limiter -> Output
```

This gives TribeBox a stable live signal flow while still allowing heavy performance manipulation through Morph Scenes, macros and FX Mode.

## Morph Scene Integration

Morph Scenes can modulate all major FX parameters.

Allowed Morph Scene FX targets:

- track filter cutoff
- track filter resonance
- track drive amount
- drive type, optional and only if safe
- delay send amount
- delay feedback
- delay filter
- delay freeze state
- reverb send amount
- reverb decay
- reverb swell amount
- master filter cutoff
- master filter mode
- stutter amount
- beat repeat rate

Example:

```text
Scene 04 - Breakdown

Morph 0%:
- groove normal
- kick full
- acid stable
- delay low
- reverb normal

Morph 50%:
- master high-pass slightly open
- acid cutoff darker
- acid delay send medium
- hat reverb higher

Morph 100%:
- kick nearly muted
- master high-pass open
- delay feedback high
- reverb decay long
- stutter prepared
```

Morph Scene modulation must be interpolated safely. Abrupt jumps should only happen where the target parameter is explicitly defined as stepped, such as filter type or drive type.

## FX Mode Mapping

In FX Mode, the 8 encoders should expose the most important live FX controls.

Suggested default page:

```text
E1 Master Filter Cutoff
E2 Master Filter Mode / Resonance
E3 Delay Feedback
E4 Delay Filter
E5 Delay Throw Amount
E6 Reverb Swell / Decay
E7 Stutter / Beat Repeat
E8 Master Morph Amount
```

Additional pages can expose deeper delay, reverb, routing and limiter parameters.

## Closed Decisions

```text
FX Engine v0.1

Per-track FX:
- Drive
- Filter
- Delay Send
- Reverb Send

Filter types:
- LP12
- LP24
- HP12
- BP12
- Notch
- Peak / Resonator
- Acid LP

Drive:
- selectable per track
- different default per track
- fixed before filter

Delay:
- one global send delay
- stereo sync
- ping pong
- feedback
- filter
- drive
- freeze

Reverb:
- one global send reverb
- plate / hall / dark hybrid
- low cut
- high cut
- decay
- freeze

Performance FX:
- Master Filter
- Stutter / Beat Repeat
- Delay Throw
- Delay Freeze
- Reverb Swell
- Safety Limiter

Routing:
- fixed performance-first routing
- Morph Scene can modulate all major FX parameters

Song Mode:
- not involved
```
