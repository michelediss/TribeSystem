# TribeBox Overview

TribeBox is the main audio instrument of Tribe System.

It is designed as a compact live groovebox for Tekno, Tribe, Hardtek and Acid performance, with a performance-first workflow inspired by dedicated hardware machines rather than DAW-style interaction.

## Main Functions

- Sequencer
- Pattern system
- Morph Scene performance layer
- Hybrid Sound Engine
- Sampler
- Internal FX engine
- OSC output

## Track Concept

- Track 1: Kick
- Track 2: Bass / Rumble
- Track 3-6: Drums and Percussion
- Track 7: Acid / Lead
- Track 8: Sampler

## Sound Engine

The v0.1 Sound Engine uses a hybrid sample+synth architecture designed for mono live sound-system use.

Core track map:

- Track 1 Kick: mono sample + drive/filter
- Track 2 Bass: mono subtractive synth with simple Rumble mode
- Track 3 Perc 1: mono sample
- Track 4 Perc 2: mono sample
- Track 5 Hat/Ride: mono sample, with future noise-hat option
- Track 6 Perc/Noise: mono sample, with future synth/noise option
- Track 7 Acid/Lead: TB-303-like mono synth with alternate Lead mode
- Track 8 Sampler: full mono sampler

Core decisions:

- drums are sample-based and performance-processed
- Bass uses subtractive mono synthesis with sub oscillator
- Acid uses TB-303-like behavior with accent, slide, Acid LP and Acid Overdrive
- Lead is an alternate Track 7 mode, not a dedicated polyphonic track
- granular, wavetable, vector and spectral synthesis are future roadmap items
- modulation is based on envelopes, 1 LFO per synth track, parameter locks and Morph Scene targets
- sound storage uses Sound Preset, Track Sound, Kit, Pattern Override and Morph Scene layers

See `docs/tribebox/audio/sound-engine.md`.

## Sample Engine

The v0.1 Sample Engine is built around Track 8 as the dedicated Sampler track and is designed exclusively for live mono sound-system use.

Core decisions:

- mono-only playback
- Track 8 Sampler scope in v0.1
- Sample Locks only on Track 8
- manual slicing and equal-grid slicing
- no realtime time-stretch in v0.1
- pitch/speed playback only
- folder-based sample browser
- sidecar `.tribe.json` metadata per sample
- project-level `sample-index.json`

See `docs/tribebox/audio/sample-engine.md`.

## Pattern System

The v0.1 pattern model is based on:

- 128 patterns per project
- 8 banks, A-H
- 16 patterns per bank
- queued pattern changes at the end of the current pattern
- Temporary Chains and Saved Chains
- no Song Mode
- 16 global Morph Scenes per project
- hybrid pattern content with shared kits and optional local overrides

See `docs/tribebox/sequencer/pattern-system.md`.

## FX Engine

The v0.1 FX engine is based on a fixed performance-first routing model:

```text
Tracks -> Per-track Drive/Filter -> Global Delay/Reverb Sends -> Performance FX -> Safety Limiter -> Output
```

Core decisions:

- per-track Drive and Filter
- one global send Delay
- one global send Reverb
- selectable drive per track with different defaults
- filter types: LP12, LP24, HP12, BP12, Notch, Peak/Resonator, Acid LP
- required Performance FX: Master Filter, Stutter / Beat Repeat, Delay Throw, Delay Freeze, Reverb Swell, Safety Limiter
- fixed routing, with Morph Scene modulation over all major FX parameters

See `docs/tribebox/audio/fx-engine.md`.

## Planned Features

- Step sequencing
- Pattern chaining
- Morph Scenes
- Euclidean sequencing
- Generative pitch engine
- Root note system
- Parameter locks
- Retrigger
- Fill
- Scale lock
