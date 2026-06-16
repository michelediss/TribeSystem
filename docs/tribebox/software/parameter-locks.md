# Parameter Locks

## Goal

Parameter locks allow individual steps to store parameter values.

## Example Targets

- Pitch
- Decay
- Filter cutoff
- Resonance
- Drive
- Volume
- Sample start
- Retrigger amount
- FX amount

## Usage

A step may override the default track value for one or more parameters.

## Priority

This is a high-impact feature, but it is also one of the more complex parts of the sequencer.

## Design Principle

Parameter locks should be powerful but fast to edit during live preparation.
