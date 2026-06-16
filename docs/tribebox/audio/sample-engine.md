# Sample Engine

Status: v0.1 closed

This document defines the TribeBox Sample Engine.

TribeBox is designed exclusively for live Tekno, Tribe, Hardtek and Acid performance. The Sample Engine must therefore prioritise stability, immediacy, predictable mono output and physical-control operation over DAW-style audio editing.

## Core Principles

- Playback is mono-only.
- The engine is designed for live sound systems, PA rigs and mono-compatible rave setups.
- Track 8 is the dedicated Sampler track in v0.1.
- Sample Locks are supported only on Track 8 Sampler.
- Slicing is supported through manual slices and equal-grid slices.
- No realtime time-stretch is required in v0.1.
- Sample browsing is folder-based only.
- Metadata is stored through sidecar `.tribe.json` files and a project-level `sample-index.json`.

## Playback Model

The v0.1 playback model is mono-only.

```text
Sample file -> Mono Decode / Mono Sum -> Sampler Voice -> Track 8 FX chain -> Mix Bus
```

Mono-only playback is a deliberate design decision:

- TribeSystem is intended exclusively for live performance.
- Large sound systems are often mono or mono-compatible in practice.
- Mono output avoids phase issues and unpredictable stereo collapse.
- CPU and memory use stay lower and more predictable.
- Live gain staging is simpler.

### Accepted File Handling

The engine may accept mono or stereo source files, but playback must be mono.

Recommended import behaviour:

```text
Mono file   -> play directly
Stereo file -> sum to mono at import or decode time
```

The project should not depend on stereo placement, stereo width or stereo modulation as a core musical feature.

## Track Scope

In v0.1, the Sample Engine is primarily bound to:

```text
Track 8: Sampler
```

Track 8 can be used for:

- vocal hits
- stabs
- sirens
- noise hits
- risers
- one-shot FX
- short loops
- sliced loops
- resampled material, future

The percussion tracks may use their own drum/synth/sample engines later, but Sample Locks and advanced slicing are scoped to Track 8 in v0.1.

## Sample Locks

Sample Locks are supported only on Track 8 Sampler.

A Sample Lock lets a single step override the track's default sample.

Example:

```text
Track 8 default sample: vocal_hit.wav

Step 01 -> vocal_hit.wav
Step 05 -> metal_stab.wav
Step 09 -> noise_riser.wav
Step 13 -> siren_fx.wav
```

This allows Track 8 to behave as a flexible live sample lane without adding more physical tracks.

### Sample Lock Data

A step-level Sample Lock should store:

- `sample_id`
- optional `slice_id`
- optional start/end override
- optional pitch/speed override
- optional gain override
- optional filter/FX parameter locks through the normal parameter-lock system

Sample Locks should reference samples by stable IDs, not by fragile absolute paths.

## Slicing

Slicing is supported in v0.1.

Supported slicing modes:

```text
Manual slicing
Equal-grid slicing
```

Not required in v0.1:

```text
Transient detection
Automatic beat detection
Warp markers
```

### Manual Slicing

Manual slicing allows the user to define slice start and end points.

Use cases:

- vocal phrase chopping
- custom break edits
- handmade texture cuts
- selecting useful regions inside long FX samples

### Equal-Grid Slicing

Equal-grid slicing divides a sample into a fixed number of equal parts.

Supported slice counts:

```text
8 slices
16 slices
32 slices
64 slices
```

Example:

```text
Sample: break_loop.wav
Grid: 16 slices

Step 01 -> Slice 01
Step 03 -> Slice 07
Step 05 -> Slice 04
Step 07 -> Slice 12
```

Equal-grid slicing is the preferred fast live workflow.

## Slice Locks

A Slice Lock lets a step select a specific slice from the currently loaded or sample-locked sample.

Example:

```text
Track 8 sample: break_loop.wav

Step 01 -> Slice 01
Step 05 -> Slice 05
Step 09 -> Slice 09
Step 13 -> Slice 13
```

Sample Locks and Slice Locks can work together:

```text
Step 01 -> break_loop.wav / Slice 01
Step 05 -> break_loop.wav / Slice 07
Step 09 -> vocal_phrase.wav / Slice 03
Step 13 -> siren.wav / full sample
```

## Time Stretch

Realtime time-stretch is not part of v0.1.

The v0.1 engine supports pitch/speed playback:

```text
Higher pitch -> faster playback
Lower pitch  -> slower playback
```

This is intentionally simpler and more predictable than realtime time-stretch.

### Loop Metadata

Samples may still store tempo information:

- original BPM
- bar length
- beat length
- root note, where useful

This allows TribeBox to show useful information and suggest playback ratios without performing realtime stretching.

Example:

```text
Sample BPM: 145
Project BPM: 150
Suggested speed ratio: +3.45%
```

### Future Time-Stretch Direction

Future versions may support offline or pre-rendered time-stretch.

Future, not v0.1:

```text
Offline time-stretch rendering
Precomputed loop variants
Stretch modes for drums / tonal / texture material
```

Realtime time-stretch should not be required for the first stable live implementation.

## Sample Browser Workflow

The v0.1 browser is folder-based only.

No tag browser, text search or database-style browsing is required in v0.1.

### Browser Entry

Recommended access:

```text
MODE -> SAMPLER
FUNC + MODE -> Sample Browser
```

### Physical Controls

The browser must be usable without touchscreen interaction.

Suggested mapping:

```text
E1              -> scroll file list
E2              -> folder navigation
E3              -> preview gain
E4              -> pitch/speed preview
E8 push         -> load selected sample
PLAY            -> preview selected sample
STOP            -> stop preview
BACK            -> exit browser
FUNC + Step     -> assign selected sample to step as Sample Lock
SHIFT + Step    -> assign selected slice to step as Slice Lock
```

### Folder Structure

Recommended sample library structure:

```text
samples/
├── kicks/
├── bass/
├── percussion/
├── hats/
├── acid/
├── vocals/
├── stabs/
├── loops/
├── textures/
├── fx/
└── resamples/
```

The browser should preserve the filesystem structure instead of forcing a custom library database.

## Metadata Storage

Metadata is stored in two layers:

```text
1. Sidecar .tribe.json per sample
2. Project-level sample-index.json
```

### Sidecar Metadata

Each sample can have a sidecar file:

```text
kick_hard_01.wav
kick_hard_01.tribe.json
```

Example:

```json
{
  "id": "sample_uuid",
  "filename": "kick_hard_01.wav",
  "type": "one-shot",
  "channels_source": 1,
  "playback_channels": 1,
  "sample_rate": 48000,
  "length_samples": 38400,
  "length_seconds": 0.8,
  "bpm": null,
  "bars": null,
  "key": null,
  "root_note": null,
  "favorite": false,
  "default_playback": {
    "mode": "one-shot",
    "output": "mono",
    "gain": 0.0,
    "pitch": 0.0,
    "start": 0,
    "end": 38400,
    "loop": false
  },
  "slices": []
}
```

Even if tags are added later, the v0.1 browser does not depend on them.

### Project Sample Index

Each project should maintain:

```text
project/sample-index.json
```

The project index is used for:

- fast loading of used samples
- resolving Sample Locks
- detecting missing samples
- preserving relative paths
- tracking project dependencies

Pattern and step data should reference samples using:

```text
sample_id
relative_path
fallback filename
```

Absolute paths should be avoided.

## Pattern Integration

Pattern data may store Track 8 sampler information:

- default sample reference
- per-step Sample Locks
- per-step Slice Locks
- per-step pitch/speed locks
- per-step start/end locks
- per-step gain locks
- per-step parameter locks for Track 8 FX

This aligns with the Pattern System hybrid content model: patterns can store sequence data, parameter locks and local overrides while still referencing shared project resources.

## Morph Scene Integration

Morph Scenes can modulate sampler parameters, but should not replace the sample browser or sample assignment workflow.

Useful Morph Scene targets:

- sample start
- sample end
- pitch/speed
- filter cutoff
- filter resonance
- drive amount
- delay send
- reverb send
- sampler level

Sample identity changes should remain explicit through Sample Locks rather than continuous morphing.

## Summary

```text
Sample Engine v0.1

Playback:
- mono-only
- live sound-system-first design
- stereo source files may be summed/imported to mono

Scope:
- Track 8 Sampler

Sample Locks:
- yes
- Track 8 only

Slicing:
- yes
- manual slicing
- equal-grid slicing
- 8 / 16 / 32 / 64 slices
- no transient detection in v0.1

Time Stretch:
- no realtime time-stretch in v0.1
- pitch/speed playback only
- BPM/bar metadata supported
- offline/pre-render time-stretch possible in future

Browser:
- folder-based only
- physical-control-first

Metadata:
- sidecar .tribe.json per sample
- project-level sample-index.json
- references by sample_id + relative path
```
