# TribeBox Sound Engine v0.1

Status: **closed for v0.1**.

The TribeBox Sound Engine is a hybrid live sound engine. It combines mono sample-based drum voices with a small number of focused mono synth voices.

TribeBox is not designed as a pure drum synth or as a general-purpose DAW-style synthesizer workstation. It is designed as a compact live groovebox for Tekno, Tribe, Hardtek and Acid performance.

Core principles:

- mono-only audio path for live sound-system use
- fixed-track sound architecture
- sample-based drums and percussion
- subtractive Bass engine
- TB-303-like Acid engine
- Track 8 full Sampler engine
- no polyphony requirement in v0.1
- performance-first modulation through parameter locks, macros and Morph Scenes
- advanced synthesis kept as a future roadmap item

## Track Sound Map

The v0.1 sound map is fixed:

| Track | Role | Sound source |
| --- | --- | --- |
| Track 1 | Kick | mono sample + drive/filter |
| Track 2 | Bass | mono subtractive synth |
| Track 3 | Perc 1 | mono sample |
| Track 4 | Perc 2 | mono sample |
| Track 5 | Hat / Ride | mono sample, future noise-hat option |
| Track 6 | Perc / Noise / Utility | mono sample, future synth/noise option |
| Track 7 | Acid / Lead | TB-303-like mono synth with alternate Lead mode |
| Track 8 | Sampler | full mono sampler |

This means that the first-generation TribeBox sound model is hybrid:

```text
Drums       = samples + processing
Bass        = subtractive mono synth
Acid        = TB-303-like mono synth
Sampler     = full mono sample engine with locks and slicing
Advanced FX = future roadmap
```

## Relationship to the Sample Engine

Sample playback is not limited to Track 8.

Tracks 1, 3, 4, 5 and 6 are sample-based drum/percussion voices. They can load and play mono samples as their primary sound source.

Track 8 is different: it is the full Sampler track.

Track 8 provides the advanced sampler functions defined in the Sample Engine specification:

- Sample Locks
- manual slicing
- equal-grid slicing
- Slice Locks
- folder-based sample browsing
- sample metadata through `.tribe.json` sidecars and `sample-index.json`

In short:

```text
Sample-based drum tracks = one primary sample voice per track sound
Track 8 Sampler          = full sample performance track
```

See `docs/tribebox/audio/sample-engine.md`.

## Track 1 Kick

Track 1 is a sample-based Kick voice.

Core architecture:

```text
Mono sample -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
```

The Kick sound is based on mono samples, with performance-oriented processing rather than a full synthesized kick engine.

Required controls:

- sample selection
- tune
- start
- decay
- amp level
- drive amount
- drive type
- filter type
- filter cutoff
- filter resonance
- delay send
- reverb send

Kick defaults should favor hardtek/tribe performance:

- mono sample playback
- strong default drive options
- stable low-end response
- no stereo dependency
- safe interaction with the Safety Limiter

## Track 2 Bass Synth

Track 2 is a mono subtractive Bass synth with a simple Rumble mode.

Core architecture:

```text
Oscillator + Sub Oscillator -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
```

Main oscillator shapes:

- Saw
- Square
- Triangle
- Sine

Sub oscillator:

- enabled on Track 2
- simple mono sub layer
- follows main pitch
- intended for low-end weight, not as a second complex oscillator

Sub oscillator types:

- Sine -1 octave
- Square -1 octave
- Sine -2 octave, optional

Filter options:

- LP12
- LP24
- HP12

Envelopes:

- Amp Envelope
- Filter Envelope
- optional Pitch Envelope

Modulation:

- 1 LFO
- parameter locks
- Morph Scene modulation
- macro encoder mapping

### Rumble Mode

Rumble mode is a simple Bass mode optimized for tribe-style low-end movement.

It is not a separate complex engine. It is a mode of the Bass synth with bass-oriented defaults and parameter ranges.

Rumble mode behavior:

- low/sub-focused bass response
- longer decay range
- drive emphasis
- filter envelope tuned for rolling bass movement
- optional pitch envelope for transient impact
- Morph Scene compatibility

Typical Rumble targets:

- filter cutoff
- filter envelope amount
- drive amount
- decay
- sub level
- delay send, used carefully

## Tracks 3 and 4 Percussion

Tracks 3 and 4 are sample-based percussion voices.

Core architecture:

```text
Mono sample -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
```

They are intended for:

- clap
- snare
- tom
- metallic percussion
- industrial hits
- tribal percussion
- short one-shot FX

Required controls:

- sample selection
- tune
- start
- decay
- amp level
- drive amount
- filter type
- filter cutoff
- filter resonance
- delay send
- reverb send

They do not require dedicated percussion synthesis in v0.1.

## Track 5 Hat / Ride

Track 5 is sample-only in v0.1.

Core architecture:

```text
Mono sample -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
```

It is intended for:

- closed hat
- open hat
- ride
- shaker
- noise-like rhythmic material

v0.1 decision:

- no dedicated noise-hat synth requirement
- mono sample playback only
- subtractive/noise hat synthesis is future roadmap

Future direction:

- simple noise-hat mode
- noise source + high-pass filter
- decay-focused envelope
- metallic/noise tone controls

## Track 6 Perc / Noise / Utility

Track 6 is sample-only in v0.1, with a future role as a utility/noise synth track.

Core architecture:

```text
Mono sample -> Drive -> Filter -> Amp -> Delay Send -> Reverb Send
```

It is intended for:

- additional percussion
- noise hits
- stabs
- siren-like samples
- utility one-shots
- short live FX material

v0.1 decision:

- no dedicated utility synth requirement
- no granular or spectral processing requirement
- mono sample playback only

Future direction:

- simple noise synth
- utility synth
- percussion synth
- riser/noise generator

## Track 7 Acid / Lead

Track 7 is the main dedicated synth track.

It has two modes:

```text
Acid Mode
Lead Mode
```

### Acid Mode

Acid Mode is TB-303-like, but not a clone requirement.

Core architecture:

```text
Oscillator + Sub Oscillator -> Accent / Slide behavior -> Acid Overdrive -> Acid LP Filter -> Amp -> Delay Send -> Reverb Send
```

Oscillator shapes:

- Saw
- Square
- Pulse

Required Acid behavior:

- accent per step
- slide per step
- gate length per step
- parameter locks
- Acid Overdrive
- Acid LP filter
- delay send
- reverb send
- Morph Scene modulation

Core Acid parameters:

- wave
- tune
- fine tune
- pulse width
- glide / slide time
- accent amount
- envelope modulation
- decay
- cutoff
- resonance
- drive
- level
- delay send
- reverb send

### Sub Oscillator on Acid

Track 7 supports a simple sub oscillator.

The sub oscillator is intentionally limited:

- follows the main oscillator pitch
- usable for heavier acid/lead lines
- does not turn the Acid engine into a dual-oscillator poly synth

Recommended sub options:

- Sine -1 octave
- Square -1 octave

Configurable behavior:

- sub level
- sub follows slide on/off

### Lead Mode

Lead Mode is an alternate Track 7 mode for mono lead lines.

It is not a separate polyphonic lead synth. It is a performance-friendly alternate behavior for the Acid/Lead track.

Lead Mode direction:

- subtractive mono lead
- less strict 303 accent/slide behavior
- optional limited FM flavor
- useful for hooks, siren-like lines, rave leads and aggressive mono melodies

Lead Mode v0.1 scope:

- no polyphony requirement
- no wavetable requirement
- no vector synthesis requirement
- no complex modulation matrix

Possible Lead oscillator set:

- Saw
- Square
- Pulse
- Triangle
- limited FM pair, optional

## Advanced Synthesis Roadmap

The following synthesis approaches are explicitly outside the v0.1 requirement:

- granular atmospheres
- wavetable pads
- vector pads
- spectral FX
- complex FM engines
- polyphonic pads
- granular resynthesis

They are kept as future roadmap directions.

Roadmap mapping:

| Sound area | Future synthesis direction |
| --- | --- |
| Hat / Ride | noise-hat engine |
| Perc / Noise / Utility | noise synth / utility synth |
| Lead | expanded subtractive + FM |
| Atmospheres | granular |
| Pads | wavetable or vector |
| FX | granular + spectral |

v0.1 remains focused on stability, mono live output and immediate performance control.

## Modulation

The v0.1 modulation model is intentionally limited and performance-first.

Confirmed modulation model:

```text
Envelopes + 1 LFO + Parameter Locks + Morph Scene modulation
```

### Envelopes

Synth tracks support:

- Amp Envelope
- Filter Envelope
- Pitch Envelope where useful

Sample-based tracks support simpler envelope behavior:

- Amp Decay
- optional start/end trimming
- optional filter envelope where useful

### LFO

Each true synth track has 1 LFO.

Applies to:

- Track 2 Bass
- Track 7 Acid / Lead
- future Track 5 noise-hat mode
- future Track 6 utility/noise synth mode

LFO waveforms:

- Sine
- Triangle
- Saw Up
- Saw Down
- Square
- Random / Sample & Hold

LFO rate:

- free rate
- tempo-synced divisions

Primary LFO destinations:

- pitch
- filter cutoff
- filter resonance
- drive amount
- pulse width
- amp level
- delay send
- reverb send

### Parameter Locks

Parameter locks are supported for sound-shaping parameters that make sense per step.

Important parameter lock targets:

- sample selection where supported
- sample tune
- sample start
- decay
- oscillator wave
- oscillator tune
- sub level
- filter cutoff
- filter resonance
- drive amount
- delay send
- reverb send
- Acid accent
- Acid slide

### Morph Scene Targets

Morph Scenes can target major sound engine parameters.

Typical targets:

- track level
- sample tune
- sample decay
- bass filter cutoff
- bass resonance
- bass drive
- bass sub level
- acid cutoff
- acid resonance
- acid drive
- acid accent amount
- acid slide time
- delay send
- reverb send

Morph Scene modulation is a performance layer, not a preset system.

## Preset, Kit and Sound Structure

The v0.1 sound storage model is:

```text
Sound Preset
Track Sound
Kit
Pattern Override
Morph Scene
```

### Sound Preset

A Sound Preset is a reusable single sound.

Examples:

```text
Kick_Distorted_01
Bass_Rumble_02
Clap_Dry_01
Perc_Metal_03
Hat_Open_909
Acid_Brutal_04
Sampler_Default
```

A Sound Preset stores:

- engine type
- sample reference or oscillator settings
- envelope settings
- filter settings
- drive settings
- LFO settings where applicable
- macro mapping
- default delay/reverb sends

### Track Sound

A Track Sound is the sound loaded on one track in the current kit.

Example:

```text
Track 7 Acid uses Acid_Brutal_04
```

### Kit

A Kit stores the 8 Track Sounds used by the project or pattern context.

Example:

```text
Kit 01 - Tribe Live Set

Track 1: Kick_Distorted_01
Track 2: Bass_Rumble_02
Track 3: Clap_Dry_01
Track 4: Perc_Metal_03
Track 5: Hat_Open_909
Track 6: Noise_Stab_01
Track 7: Acid_Brutal_04
Track 8: Sampler_Default
```

### Pattern Override

A Pattern Override lets a pattern change selected sound parameters without duplicating the entire kit.

Example:

```text
Pattern A01:
Track 7 Acid cutoff base = 45

Pattern A02:
Track 7 Acid cutoff base = 70
Track 7 Acid drive = +20%
```

This matches the Pattern System decision that patterns use hybrid content: sequence plus main pattern parameters, shared kit/sound reference and optional local overrides.

### Morph Scene

A Morph Scene is not a Sound Preset.

It is a live transformation layer above the kit and pattern.

```text
Sound Preset = sound identity
Kit          = 8 loaded track sounds
Pattern      = sequence plus optional overrides
Morph Scene  = live transformation
```

## Summary

Sound Engine v0.1:

```text
Architecture:
- hybrid sample + synth
- mono-only
- fixed-track design
- performance-first
- no polyphony requirement in v0.1

Sample-based tracks:
- Track 1 Kick
- Track 3 Perc 1
- Track 4 Perc 2
- Track 5 Hat/Ride
- Track 6 Perc/Noise
- Track 8 Sampler

Synth tracks:
- Track 2 Bass
- Track 7 Acid / Lead

Bass:
- subtractive mono synth
- sub oscillator
- simple Rumble mode

Acid:
- TB-303-like
- accent
- slide
- Acid LP
- Acid Overdrive
- sub oscillator

Lead:
- alternate mode on Track 7
- subtractive + limited FM flavor

Modulation:
- envelopes
- 1 LFO per synth track
- parameter locks
- Morph Scene targets

Preset structure:
- Sound Preset
- Track Sound
- Kit
- Pattern Override
- Morph Scene

Future:
- noise-hat engine
- utility/noise synth
- granular atmospheres
- wavetable/vector pads
- spectral FX
```
