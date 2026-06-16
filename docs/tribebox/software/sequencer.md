# Sequencer

## Goal

The sequencer is the core timing and event engine of TribeBox.

## Planned Features

- 8 tracks
- 16 / 32 / 64 step patterns
- Independent track length
- Pattern chaining
- Mute and solo
- Fill
- Retrigger
- Trig conditions
- Parameter locks

## Trigger Engine

Trigger generation should be separated from pitch generation.

Planned trigger modes:

- Step
- Euclidean

## Pitch Engine

Pitch logic is handled separately.

Planned pitch modes:

- Manual
- Scale lock
- Generative

## Design Principle

Rhythm and pitch should be independent layers.
