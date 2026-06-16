# TribeBox Parameter Locks

Status: **defined**

Parameter Locks are the per-step override system used by the TribeBox Sequencer Engine.

They are inspired by the Elektron workflow, but constrained for TribeBox's fixed-track, mono, live-performance architecture.

---

## Purpose

A Parameter Lock lets one step temporarily override a Track Sound, Kit, or Pattern value without changing the base sound.

Example:

```text
Track 1 Kick base drive: 20
Step 09 p-lock drive: 60
```

Only Step 09 uses drive 60. The Kick Track Sound remains unchanged.

---

## Core Rules

- Parameter Locks are stored inside step data.
- Parameter Locks are non-destructive relative to the Track Sound and Kit.
- Parameter Locks only affect playback of the step where they are placed.
- The first implementation supports a maximum of 16 locked parameters per step.
- Lockable parameters are controlled by a whitelist.
- Unsupported or invalid locks should be ignored safely and surfaced as warnings in debug or advanced UI.

---

## Resolution Order

Parameter values are resolved in this order:

```text
Factory Default
→ Sound Preset
→ Track Sound
→ Kit
→ Pattern Variation
→ Step Parameter Locks
→ Accent / Slide / Retrig modifiers
→ Performance Macros
→ Morph Scene
→ Mixer
→ Master Safety Limiter
```

Step Parameter Locks override the base pattern/track value, but performance layers may still transform the final result.

The Master Safety Limiter remains final.

---

## Generic Lockable Parameters

All tracks may support these lock categories where musically relevant:

```text
level
pitch / tune
decay / release
filter_cutoff
filter_resonance
drive
delay_send
reverb_send
gate / length
probability
retrig settings
microtiming
accent
```

Not every track must expose every parameter in the UI. The whitelist defines what the engine accepts; each track UI may show only the parameters that make sense.

---

## Sample Track Locks

Sample-based tracks may additionally support:

```text
sample_slot
sample_start
sample_end
loop_enabled
reverse
slice
playback_mode
```

`playback_mode` is only lockable if the Sample Engine supports it safely for that track type.

Track 8 Sampler supports the deepest sample-lock and slice-lock behavior.

---

## Track 8 Sampler Lock Resolution

Track 8 resolves sample-related locks in this order:

```text
Track 8 default sample
→ step sample_lock, if present
→ step slice_lock, if present
→ sample_start / sample_end locks
→ pitch / filter / envelope locks
→ FX send locks
```

This allows a step to select a different sample, select a slice within that sample, then apply shaping and FX locks.

Example:

```json
{
  "active": true,
  "sample_lock": "samples/breakbeat_01.wav",
  "slice_lock": {
    "mode": "manual",
    "slice_id": "slice_04"
  },
  "locks": {
    "pitch": -12,
    "filter_cutoff": 64,
    "delay_send": 20
  }
}
```

---

## Kick Locks

Track 1 Kick may support:

```text
level
tune
decay
filter_cutoff
filter_resonance
drive
transient / click emphasis, if available
delay_send
reverb_send
accent
microtiming
retrig
probability
```

Accent may increase level, transient, drive, and filter openness depending on the Kick Track Sound.

---

## Bass Locks

Track 2 Bass may support:

```text
note
octave
waveform
sub_level
filter_cutoff
filter_resonance
filter_env_amount
amp_decay / release
drive
glide_amount, if enabled
delay_send
reverb_send
accent
microtiming
retrig
probability
```

Bass can use either `CHROMATIC` or `SCALE_LOCKED` note behavior from the Pattern musical context.

---

## Percussion Sample Track Locks

Tracks 3–6 may support:

```text
sample_slot
sample_start
sample_end
pitch
level
decay
filter_cutoff
filter_resonance
drive
reverse, if supported
loop_enabled, if supported
delay_send
reverb_send
accent
microtiming
retrig
probability
```

These tracks should remain simpler than Track 8.

---

## Hat / Ride Track Locks

Track 5 Hat/Ride may support the standard sample-track locks, plus future noise-hat parameters if the future noise-hat mode is implemented.

Potential future locks:

```text
noise_color
noise_decay
metallic_amount
```

These are not required for v0.1.

---

## Perc / Noise Track Locks

Track 6 Perc/Noise may support the standard sample-track locks, plus future synthetic noise parameters if the future synth/noise mode is implemented.

Potential future locks:

```text
noise_color
noise_decay
texture_amount
```

These are not required for v0.1.

---

## Acid Locks

Track 7 Acid may support:

```text
note
octave
accent
slide
slide_time, optional
filter_cutoff
filter_resonance
env_mod
decay
drive
level
delay_send
reverb_send
microtiming
retrig
probability
```

Acid accent and slide are first-class step properties.

Slide belongs to the destination step.

```text
Step 01: C2
Step 02: D2 + slide
```

Result:

```text
C2 slides into D2
```

---

## Morph Scene Interaction

Morph Scenes operate above Parameter Locks.

This means:

```text
p-locked value → Morph Scene transform → Mixer/Master output
```

Example:

```text
Step 09 filter_cutoff p-lock: 70
Morph Scene closes filter by -20
Final effective cutoff: 50
```

Morph Scenes should not rewrite stored p-locks unless the user explicitly commits a performance state in a future workflow.

---

## Performance Macro Interaction

Performance Macros operate above Parameter Locks and below Morph Scenes.

```text
Step Parameter Locks
→ Accent / Slide / Retrig modifiers
→ Performance Macros
→ Morph Scene
```

Macros should be able to push or scale locked values, but not destructively change the stored step data unless explicitly committed.

---

## Invalid Lock Handling

A Parameter Lock can become invalid if:

- a sample is missing
- a slice map changes
- a track changes mode
- a parameter is deprecated by a file-format migration
- a sound engine no longer supports the parameter

Fallback behavior:

```text
invalid lock → ignore lock for playback → keep serialized value → show warning where possible
```

The pattern should remain loadable and playable.

---

## Storage Recommendation

Locks should be stored as sparse key/value data.

Example:

```json
{
  "locks": {
    "filter_cutoff": 72,
    "drive": 18,
    "delay_send": 24
  }
}
```

Do not store every possible parameter on every step. Only store deviations from the base value.

---

## Final Decision

TribeBox supports deep but whitelisted per-step Parameter Locks. Locks are sparse, non-destructive, limited to 16 parameters per step in the first implementation, resolved after Pattern Variation data, and transformed later by performance layers such as Macros and Morph Scenes.
