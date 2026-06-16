# TribeBox Sequencer Engine

Status: **defined**

The TribeBox Sequencer Engine follows an Elektron-inspired step sequencing model adapted to TribeBox's mono, live, sound-system-oriented workflow.

The guiding principle is:

> Simple patterns, intelligent steps, non-destructive performance.

The sequencer is not intended to become a DAW-like automation system. It is designed around fixed musical tracks, expressive per-step behavior, fast live editing, and safe recovery during performance.

---

## Core Model

TribeBox uses:

- 8 fixed tracks
- 16-step pages
- up to 128 steps per pattern
- per-track length
- separate Trigger Engine and Pitch Engine layers
- STEP and EUCLIDEAN trigger generation
- OFF / MANUAL / ROOT_LOCK / SCALE_LOCK / GENERATIVE pitch modes
- per-step parameter locks
- per-step sample locks
- per-step slice locks on Track 8 Sampler
- accent handling
- Acid slide handling
- microtiming
- swing
- retrig / ratchet
- fill behavior
- trig conditions
- probability
- shared root/scale musical context
- controlled generative pitch behavior
- non-destructive pattern variations

The sequencer should feel immediate like a drum machine, but deep enough to create evolving patterns from a small amount of visible step data.

The Trigger Engine and Pitch Engine are intentionally separate:

```text
Trigger Engine = when a sound is triggered
Pitch Engine   = which note is played
```

See `trigger-pitch-engine.md` for the detailed Trigger / Pitch Engine specification.

---

## Pattern Structure

A Pattern contains one shared Kit reference, one musical context, and up to four Variation slots.

```text
Pattern
├─ kit_reference
├─ musical_context
│  ├─ root_note
│  └─ scale
├─ global_swing
├─ master_length_steps
├─ Variation A
├─ Variation B
├─ Variation C
└─ Variation D
```

Each Variation stores sequencer data, while Kit and Track Sounds remain shared unless explicitly overridden in a future workflow.

```text
Variation
├─ tracks[8]
│  ├─ track_length_steps
│  ├─ track_swing_amount
│  ├─ trigger_engine
│  ├─ pitch_engine
│  └─ steps[128]
└─ variation_metadata
```

Default pattern length is 16 steps. Maximum pattern length is 128 steps.

---

## Step Sequencing Model

The UI model is based on 16 trig buttons.

- 16 steps are visible per page.
- A Pattern may use 1 to 8 pages.
- The base grid is 1/16 notes.
- Each track may have an independent length from 1 to 128 steps.
- Pattern changes are quantized to the master pattern boundary.

The default behavior should be simple:

```text
master_length_steps: 16
track_length_steps: inherit master length
trigger_engine.mode: STEP
```

Polymeter is enabled by changing track lengths independently.

STEP mode uses the visible grid directly. EUCLIDEAN mode may generate trigger placement from `steps`, `hits`, and `rotation`, but it only decides when a sound plays. It does not decide which note is played.

---

## Trigger / Pitch Engine Separation

TribeBox must not confuse rhythm generation with note generation.

Every track can be understood as two independent layers:

```text
Track
├─ Trigger Engine
│  └─ when the sound plays
└─ Pitch Engine
   └─ which note or pitch is used
```

Example:

```text
Track 7 Acid
Trigger Engine = EUCLIDEAN
Pitch Engine   = GENERATIVE
Root           = E
Scale          = phrygian
```

Meaning:

```text
The rhythm is Euclidean.
The notes are generated inside E phrygian.
```

Initial Trigger Engine modes:

```text
STEP
EUCLIDEAN
```

Initial Pitch Engine modes:

```text
OFF
MANUAL
ROOT_LOCK
SCALE_LOCK
GENERATIVE
```

This allows combinations such as:

```text
Kick: Step + Root Lock
Bass: Euclidean + Generative
Acid: Step + Scale Lock
Perc: Euclidean + Off
Stab: Step + Root Lock
```

Detailed behavior, per-track recommendations, sample metadata requirements, and playback resolution order are defined in `trigger-pitch-engine.md`.

---

## Step Data Model

A step may contain trigger state, note data, timing offsets, conditions, probability, performance flags, sample/slice locks, retrig settings, and parameter locks.

In STEP trigger mode, `active` is the direct grid trigger state.

In EUCLIDEAN trigger mode, generated trigger placement may be resolved into the same playback event model, while preserving the Euclidean engine parameters at track level.

Example generic step:

```json
{
  "active": true,
  "note": 36,
  "velocity": 100,
  "length": 0.75,
  "microtiming": 0,
  "condition": "ALWAYS",
  "probability": 100,
  "accent": false,
  "slide": false,
  "sample_lock": null,
  "slice_lock": null,
  "retrig": {
    "enabled": false,
    "count": 2,
    "decay": 0,
    "length": "short"
  },
  "locks": {
    "filter_cutoff": 72,
    "drive": 18,
    "delay_send": 24
  }
}
```

Example Track 8 Sampler step:

```json
{
  "active": true,
  "sample_lock": "samples/vocal_chop_03.wav",
  "slice_lock": {
    "mode": "manual",
    "slice_id": "slice_07"
  },
  "pitch": -12,
  "microtiming": -8,
  "condition": "2:4",
  "probability": 80,
  "locks": {
    "sample_start": 0.21,
    "sample_end": 0.34,
    "filter_cutoff": 64,
    "reverb_send": 18
  }
}
```

---

## Parameter Locks

Parameter locks allow a step to override selected Track Sound or Kit values.

Parameter locks are deep but controlled by a whitelist. The first implementation should support a maximum of 16 locked parameters per step.

The general supported lock categories are:

- level
- pitch / tune
- decay / release
- filter cutoff
- filter resonance
- drive
- delay send
- reverb send
- gate / length
- probability
- retrig settings
- microtiming
- accent

Sample tracks additionally support:

- sample slot
- sample start
- sample end
- loop on/off
- reverse
- slice
- playback mode, where supported by the Sample Engine

Bass additionally supports:

- note
- octave
- waveform
- filter envelope amount
- amp envelope
- sub level
- glide amount, if enabled

Acid additionally supports:

- note
- octave
- accent
- slide
- cutoff
- resonance
- envelope modulation
- decay
- drive

See `parameter-locks.md` for the detailed parameter-lock behavior.

---

## Track 8 Sample Lock Interaction

Track 8 is the full mono sampler track and supports complete per-step sample locking.

Resolution order:

```text
Track 8 default sample
→ step sample_lock, if present
→ step slice_lock, if present
→ sample start/end locks
→ pitch/filter/envelope locks
→ FX send locks
```

A `sample_lock` only affects the step on which it is placed. It does not modify the Track Sound or default sample assignment.

Example:

```text
Track 8 default sample: vocal_loop.wav

Step 01: default vocal_loop.wav
Step 05: sample_lock → siren.wav
Step 09: sample_lock → stab.wav
Step 13: default vocal_loop.wav
```

Tonal sample locks may additionally use sample metadata such as `root_note` and `sample_type` for Root Lock or Scale Lock behavior. The detailed rule is defined in `trigger-pitch-engine.md`.

---

## Track 8 Slice Lock Interaction

Track 8 supports three sampler modes:

```text
ONE_SHOT
SLICE
LOOP
```

In `SLICE` mode, each step may lock a specific slice.

```text
Step 01 → slice 01
Step 02 → slice 05
Step 03 → slice 03
Step 04 → slice 12
```

If both sample lock and slice lock are present, the sample lock is resolved first and the slice lock is applied to that selected sample.

If the selected sample has no valid slice map, the fallback behavior is:

```text
invalid slice_lock → play selected sample from the beginning
```

The UI should mark the step as having an orphan or invalid slice lock, but playback should remain safe and predictable.

---

## Accent Handling

Accent is a step property, not merely velocity.

Each step may have:

```text
accent: off / on
```

Each track has:

```text
accent_amount: 0–100%
```

Accent keeps a different musical meaning depending on the track type.

### Kick

Accent may increase:

- level
- transient / click emphasis, if available
- drive
- filter openness

### Bass

Accent may increase:

- amp level
- filter envelope amount
- perceived attack
- sub emphasis, if available

### Acid

Accent follows a TB-303-like behavior:

- higher level
- stronger filter envelope amount
- more pronounced decay
- more aggressive resonance / drive, where supported

### Sample Tracks

Accent may increase:

- level
- filter openness
- drive, if enabled
- send amount, if enabled in the Track Sound

Velocity and accent remain separate concepts:

```text
velocity = base trigger intensity
accent = musical/performance emphasis
```

---

## Acid Slide Handling

Slide is supported by Acid/Lead mode and may optionally be reused by Bass as glide.

For Acid:

```text
slide: off / on
```

Slide belongs to the destination step.

```text
Step 01: C2
Step 02: D2 + slide
```

Result:

```text
C2 slides into D2
```

If two Acid notes are consecutive and the destination step has slide enabled:

- pitch transition is continuous
- the envelope is not fully retriggered
- accent and slide may coexist
- slide time is defined at track level and may later be parameter-locked

Initial parameters:

```text
acid_slide_time: 0–100%
acid_slide_shape: linear / exponential
```

For v0.1, `acid_slide_time` is enough.

---

## Microtiming

Microtiming is stored as microticks inside each 1/16 step.

```text
1 step = 1/16 note
1 step = 96 microticks
microtiming range = -48 … +47 microticks
```

The UI may expose this as:

```text
EARLY ← CENTER → LATE
```

Advanced views may expose the numeric microtick value.

The storage format remains independent from the audio scheduler implementation. The audio engine may schedule events sample-accurately or block-accurately, but the pattern format stores microtiming as integer microticks.

---

## Swing

Swing exists at Pattern level and can be scaled per track.

Pattern:

```text
global_swing: 50–75%
```

Track:

```text
track_swing_amount: inherit / 0–100%
```

`50%` means no swing.

Timing resolution order:

```text
grid position
→ swing offset
→ microtiming offset
→ retrig offsets
```

Microtiming is always an adjustment on top of the swing result.

---

## Retrig / Ratchet

Retrig is stored per step.

```text
retrig_enabled: true / false
retrig_count: 2 / 3 / 4 / 6 / 8 / 12 / 16
retrig_decay: 0–100%
retrig_length: gate / full / short
```

Condition and probability are evaluated before retrig generation.

```text
if condition/probability fails → no trigger, no retrig
if condition/probability passes → generate retrig events
```

Retrig does not create hidden steps. It is a playback modifier for the current step.

---

## Fill

Fill is a special performance condition.

Supported fill-related conditions:

```text
FILL
NOT_FILL
```

The Fill button supports two modes:

```text
momentary: active while held
latched: active until the end of the current pattern cycle
```

Example:

```text
Step 15 snare roll → condition: FILL
Step 16 crash      → condition: FILL
Step 16 normal hat → condition: NOT_FILL
```

Fill should not require a separate Pattern or Variation.

---

## Trig Conditions and Probability

Trig conditions and probability are separate.

Each step has:

```text
condition: ALWAYS | FILL | NOT_FILL | FIRST | NOT_FIRST | PRE | NOT_PRE | ratio condition
probability: 0–100%
```

Evaluation order:

```text
condition
→ probability
→ trigger playback
→ retrig generation
```

Example:

```text
condition: 1:4
probability: 70%
```

The step is eligible only on the first cycle out of four. On that eligible cycle, it has a 70% chance of playing.

See `trig-conditions.md` for the detailed condition set.

---

## Track Length and Polymeter

Each track may use an independent length from 1 to 128 steps.

```text
track_length_steps: 1–128
```

The Pattern also has a master length:

```text
master_length_steps: 16 / 32 / 48 / 64 / 128
```

Example:

```text
Kick: 16 steps
Bass: 32 steps
Hat: 15 steps
Acid: 64 steps
Sampler: 23 steps
```

Pattern changes are quantized to the master pattern boundary, not to each individual track loop.

---

## Musical Context, Root Lock, Scale Lock, and Generative Pitch

Each Pattern defines a musical context:

```text
root_note
scale
```

This context is used by tonal Pitch Engine modes:

```text
ROOT_LOCK
SCALE_LOCK
GENERATIVE
```

Initial scale set:

```text
chromatic
major
natural_minor
minor_pentatonic
dorian
phrygian
```

Default:

```text
root_note: C
scale: chromatic
```

Root Lock, Scale Lock, and Generative pitch behavior are not the same feature:

| Mode | Meaning |
| --- | --- |
| ROOT_LOCK | Anchors a sound to the project/pattern root |
| SCALE_LOCK | Corrects entered or produced notes into the selected scale |
| GENERATIVE | Creates note choices inside musical rules |

The full architecture is defined in `trigger-pitch-engine.md`.

Important rule:

```text
Scale Lock corrects notes.
Generative creates notes.
Root Lock anchors tonal material to the root.
```

For live reliability, the first implementation should keep generative pitch behavior explicit, recallable, and easy to inspect. Invisible uncontrolled playback mutation is not part of the default v0.1 behavior.

---

## Pattern Variation Workflow

Each Pattern has four Variation slots:

```text
A
B
C
D
```

Suggested use:

- A = main groove
- B = alternate groove or bassline
- C = breakdown
- D = fill / destroy / performance version

A Variation stores sequencer data:

- step data
- trigger engine settings
- pitch engine settings
- track length
- trig conditions
- probability
- parameter locks
- sample locks
- slice locks
- note data
- retrig settings

The Kit remains shared unless a future explicit override workflow is introduced.

Variation changes are quantized to the Pattern boundary by default. A future performance option may allow quantization to the next bar.

---

## Non-Destructive Performance Layer

Live performance edits should not immediately destroy the saved Pattern state.

The engine should support:

```text
Reload Pattern
Reload Kit
Clear Performance Layer
Commit Variation
```

Performance macros and Morph Scenes operate above step data. They can temporarily alter the sound and mix state while preserving a safe return point.

---

## Value Resolution Order

When multiple layers affect the same parameter, values are resolved in this order:

```text
Factory Default
→ Sound Preset
→ Track Sound
→ Kit
→ Pattern Variation
→ Trigger Engine
→ Pitch Engine
→ Step Parameter Locks
→ Accent / Slide / Retrig modifiers
→ Performance Macros
→ Morph Scene
→ Mixer
→ Master Safety Limiter
```

The Trigger Engine decides whether an event exists. The Pitch Engine only resolves note or pitch data for events that actually trigger.

The Master Safety Limiter remains final and should not be bypassed accidentally.

---

## Development Phases

### Phase 1 — Core Step Engine

- 8 tracks
- 16/32/64 step patterns
- note on/off
- velocity
- track length
- mute
- pattern clock
- basic sample/synth triggering
- Trigger Engine mode: STEP
- Pitch Engine modes: OFF / MANUAL

### Phase 2 — Elektron Core

- parameter locks
- sample locks
- Track 8 slice locks
- accent
- Acid slide
- microtiming
- swing
- Root Lock
- Scale Lock

### Phase 3 — Variation Engine

- trig conditions
- probability
- retrig
- fill
- Variation A/B/C/D
- reload pattern
- Euclidean Trigger Engine

### Phase 4 — Controlled Generative Pitch and Performance Layer

- Generative Pitch Engine
- root/scale/range/density/probability/evolution parameters
- Morph Scenes
- macro layer
- safe reset
- panic / clean state
- commit / reload workflow

---

## Final Decision

The TribeBox Sequencer Engine is defined as an Elektron-inspired step sequencer with 8 fixed tracks, 16-step pages, up to 128 steps per pattern, per-track length, separate Trigger Engine and Pitch Engine layers, STEP and EUCLIDEAN trigger modes, OFF / MANUAL / ROOT_LOCK / SCALE_LOCK / GENERATIVE pitch modes, deep but whitelisted parameter locks, sample and slice locks for Track 8, accent, Acid slide, microtiming, swing, retrig, fill, trig conditions, probability, shared root/scale context, controlled generative pitch behavior, and non-destructive pattern variations.

The engine prioritizes live reliability, mono sound-system performance, readable musical behavior, and fast recovery over unrestricted DAW-style automation.
