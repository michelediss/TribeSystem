# TribeBox Overview

TribeBox is the main audio instrument of Tribe System.

It is designed as a compact live groovebox for Tekno, Tribe, Hardtek and Acid performance, with a performance-first workflow inspired by dedicated hardware machines rather than DAW-style interaction.

## Main Functions

- Sequencer
- Pattern system
- Morph Scene performance layer
- Sampler
- Synth engine
- Internal FX engine
- OSC output

## Track Concept

- Track 1: Kick
- Track 2: Bass / Rumble
- Track 3-6: Drums and Percussion
- Track 7: Acid / Lead
- Track 8: Sampler

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
