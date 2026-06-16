# Pattern System

This document defines the intended TribeBox pattern architecture.

TribeBox is a performance-first groovebox. The pattern system must support fast live interaction, reliable pattern switching and expressive transformations without turning the device into a DAW-style arranger.

## Pattern System Status

```text
Status: v0.1 closed
Primary mode: Pattern Mode
Arrangement model: Pattern Chain
Song Mode: absent
Scene model: global Morph Scenes
```

## Core Principles

- Patterns are the main musical building blocks of a live set.
- Pattern switching must be predictable and safe during performance.
- Pattern chains should support live structure without requiring a full song arranger.
- Morph Scenes are a mandatory performance layer above patterns.
- Song Mode is intentionally excluded from v0.1.
- The system must remain usable from physical controls, with the display used as feedback.

## Pattern Capacity

Each project contains 128 patterns.

```text
8 banks × 16 patterns = 128 patterns per project
```

Banks are named from `A` to `H`.

```text
Bank A: A01–A16
Bank B: B01–B16
Bank C: C01–C16
Bank D: D01–D16
Bank E: E01–E16
Bank F: F01–F16
Bank G: G01–G16
Bank H: H01–H16
```

## Pattern Mode

Pattern selection happens in `PATTERN MODE` using the 16 RGB step buttons.

```text
Step 1–16    = select or queue pattern in the current bank
PAGE         = next bank
SHIFT + PAGE = previous bank
```

Example with Bank A active:

```text
[01] = A01
[02] = A02
[03] = A03
...
[16] = A16
```

The display should clearly show the current bank, current pattern and queued pattern.

```text
Bank:    A
Current: A03
Queued:  A04
Change:  end of pattern
```

## Pattern Switching

The default pattern change behavior is queued switching at the end of the current pattern.

```text
Press pattern button
↓
Selected pattern is queued
↓
Current pattern finishes
↓
Queued pattern starts
```

This avoids accidental hard cuts during live performance.

An optional force-change shortcut can be considered as a protected secondary action.

```text
SHIFT + Pattern = force immediate change
```

This shortcut should be configurable or disabled by default to avoid live mistakes.

## Pattern Chains

TribeBox supports both Temporary Chains and Saved Chains.

Pattern Chains are not Song Mode. They are performance tools for sequencing patterns without creating a full linear arrangement timeline.

### Temporary Chain

Temporary Chain is the live chaining workflow.

Example:

```text
A01 → A02 → A03 → A02 → A04
```

Proposed physical workflow:

```text
PATTERN MODE
FUNC + Step   = add pattern to temporary chain
BACK          = remove last chain entry
SHIFT + BACK  = clear temporary chain
```

The chain can loop until cleared or replaced.

The display should show the active chain, the next pattern and the loop state.

```text
Chain: A01 > A02 > A03 > A02 > A04
Next:  A02
Loop:  on
```

### Saved Chains

Saved Chains are reusable pattern sequences stored inside the project.

```text
16 Saved Chains per project
up to 64 pattern entries per chain
```

Example:

```text
Chain 01: Intro
A01 × 4
A02 × 4
A03 × 8

Chain 02: Main Groove
B01 × 8
B02 × 8
B03 × 4
B04 × 4
```

A chain entry should support at least:

```text
pattern reference
repeat count
optional default Morph Scene override
optional initial Morph Amount override
```

Saved Chains should remain secondary to live Pattern Mode. They are useful for recurring sections such as intro, buildup, main groove, break, drop and outro, but should not impose a rigid arrangement workflow.

## Song Mode

Song Mode is intentionally absent.

```text
Song Mode: not planned for v0.1
```

TribeBox should not provide a full DAW-like arranger with a fixed timeline. The intended structure is:

```text
Pattern
Pattern Chain
Morph Scene
Mute state
Performance macros
```

This keeps the device focused on live Tekno, Tribe, Hardtek and Acid performance.

## Morph Scene System

TribeBox uses 16 global Morph Scenes per project.

```text
16 global Morph Scenes per project
```

Morph Scenes are global because scene-per-pattern storage would become too complex in live use. With 128 patterns, 16 scenes per pattern would create up to 2048 scene states, which would be difficult to manage during performance.

Global Morph Scenes provide a stable set of performative transformations available across the whole project.

Example scene roles:

```text
Scene 01: Clean Groove
Scene 02: Acid Open
Scene 03: Kick Distortion
Scene 04: Breakdown
Scene 05: Delay Feedback Rise
Scene 06: Percussion Chaos
Scene 07: Dark Filtered
Scene 08: Drop
Scene 16: Emergency Reset / Clean State
```

## Pattern and Morph Relationship

Each pattern can store:

```text
default Morph Scene
initial Morph Amount
```

Example:

```text
Pattern A01 → Scene 01, Morph 0%
Pattern A02 → Scene 02, Morph 35%
Pattern A03 → Scene 04, Morph 60%
Pattern A04 → Scene 08, Morph 100%
```

The Morph Scene layer should not destructively replace the pattern. It acts as a performance transformation layer above the current pattern.

Example:

```text
Pattern A03 = current groove

Scene 04 = breakdown state:
- kick volume down
- acid cutoff closed
- delay send high
- reverb length increased
- hats filtered

Morph 0%   = original pattern
Morph 50%  = partial transition
Morph 100% = full scene transformation
```

Encoder `E8` should remain the main Master Morph control.

```text
E8 = Master Morph Amount
```

## Pattern Content Model

TribeBox uses a hybrid pattern content model.

A pattern stores sequence and main pattern-level data, while sound identity can be shared through kits.

```text
Pattern = sequence + main pattern parameters + kit reference + optional local overrides
Kit     = reusable sound state for the 8 fixed tracks
```

### Project Content

A project contains:

```text
banks
patterns
kits
Morph Scenes
Saved Chains
global settings
samples
```

### Kit Content

A kit contains shared sound state for the 8 fixed tracks.

```text
Track 1: Kick engine
Track 2: Bass engine
Track 3: Perc 1 engine
Track 4: Perc 2 engine
Track 5: Hat / Ride engine
Track 6: Perc 3 / Noise / Utility engine
Track 7: Acid Synth engine
Track 8: Sampler engine

FX routing
base mixer state
macro mapping
scene mapping targets
```

### Pattern Content

A pattern contains:

```text
step sequence
trigs
notes
velocity / accent
microtiming
probability
ratchet / retrig
parameter locks
track length
pattern length
swing
default kit reference
optional local sound overrides
default Morph Scene
initial Morph Amount
```

This hybrid model allows consistent sound identity across a live set while still allowing pattern-specific changes when needed.

## Physical Control Mapping

### Pattern Mode

```text
Step 1–16       = select or queue pattern in current bank
PAGE            = next bank
SHIFT + PAGE    = previous bank
FUNC + Step      = add pattern to temporary chain
BACK            = remove last temporary chain entry
SHIFT + BACK    = clear temporary chain
REC + Step       = capture or save current pattern slot
SHIFT + REC      = duplicate or copy pattern
```

### Morph Scene Mode

```text
Step 1–16       = select Morph Scene
SHIFT + Step    = save current state into scene slot
E1–E7           = scene macro controls / assigned morph dimensions
E8              = Master Morph Amount
PAGE            = scene parameter page
BACK            = return to Performance Dashboard
```

## Display Requirements

Pattern Mode display should expose the live state without hiding critical information.

Required information:

```text
current bank
current pattern
queued pattern
active chain state
next chain entry
chain loop state
active Morph Scene
Morph Amount
```

Suggested dashboard section:

```text
Bank A | Current A03 | Queued A04
Chain: A01 > A02 > A03 > A04 | Loop on
Scene: 04 Breakdown | Morph 67%
```

## Closed v0.1 Decisions

```text
Patterns per project: 128
Banks: 8 banks, A–H
Patterns per bank: 16
Pattern names: A01–H16
Pattern selection: PATTERN MODE + 16 step buttons
Pattern switching: queued at end of current pattern
Pattern chaining: Temporary Chain + Saved Chain
Saved Chains: 16 per project, up to 64 entries each
Song Mode: absent
Morph Scene model: 16 global Morph Scenes per project
Pattern + Morph relation: default scene + initial morph amount per pattern
Pattern content: hybrid, with shared kits and optional local overrides
```
