# Workflow

This document defines the intended TribeBox interaction workflow.

TribeBox is a compact live groovebox for Tekno, Tribe, Hardtek and Acid performances. The workflow is performance-first and inspired by dedicated hardware grooveboxes rather than DAW-style interaction.

## Workflow Status

```text
Status: v0.1 closed
Primary use: live performance
Primary input: physical controls
Display role: feedback and setup support
```

## Core Principles

- The performer must be able to play a full live set without relying on the touchscreen.
- Common live actions must be fast and physical.
- The display must clearly explain what the controls are doing.
- Shift and function layers must remain usable under live pressure.
- Destructive actions must require confirmation, long press or a protected shortcut.

## Boot Workflow

On startup, TribeBox should open directly into the live performance state.

```text
Power on
↓
Load last project/session
↓
Open Performance Dashboard
↓
Ready to play
```

The device should not boot into a setup screen, file browser or technical menu.

## Main Modes

The MODE button cycles or opens the main mode selector.

Planned modes:

```text
PERFORMANCE
TRACK
MIXER
FX
MORPH SCENE
PATTERN
PROJECT / SYSTEM
```

PAGE changes the page inside the current mode.

Example:

```text
TRACK mode
PAGE 1 = main sound parameters
PAGE 2 = envelope and modulation
PAGE 3 = routing and sends
PAGE 4 = advanced parameters
```

## Global Buttons

The fixed global buttons are:

```text
PLAY
STOP
REC
SHIFT
FUNC
MODE
PAGE
BACK
```

### Modifier Rules

SHIFT is a momentary hold modifier.

```text
SHIFT = hold only
FUNC = secondary function/menu layer
Long press = protected or destructive action
```

SHIFT should not behave as a toggle by default. This reduces accidental live mistakes.

## Performance Mode

Performance mode is the default mode.

In Performance mode:

- Step buttons edit or trigger steps for the selected track.
- Encoders control 8 live macro parameters.
- The display shows the Performance Dashboard.
- The selected track remains visible at all times.
- Morph scene status remains visible at all times.

Common one-click actions:

- Start transport
- Stop transport
- Enter recording
- Toggle steps
- Tweak macro values
- Change page

Common two-action workflows:

- Select track
- Mute/unmute tracks
- Change pattern
- Recall morph scene
- Save morph scene

## Track Selection Workflow

There are no dedicated track buttons in v0.1.

Track selection uses the 16 step buttons in a dedicated track selection state.

```text
Enter Track Select
↓
Step buttons 1-8 select tracks 1-8
↓
Return to previous performance context
```

Fixed tracks:

1. Kick
2. Bass
3. Perc 1
4. Perc 2
5. Hat / Ride
6. Perc 3 / Noise / Utility
7. Acid Synth
8. Sampler

## Mute Workflow

Mute mode uses the first 8 step buttons.

```text
Mute Mode
1 = Kick mute
2 = Bass mute
3 = Perc 1 mute
4 = Perc 2 mute
5 = Hat/Ride mute
6 = Perc 3/Noise mute
7 = Acid Synth mute
8 = Sampler mute
```

RGB feedback should clearly show:

- Active tracks
- Muted tracks
- Selected track
- Currently sounding steps when possible

## Step Editing Workflow

Basic step editing:

```text
Step button = toggle trig on/off
SHIFT + Step = clear trig or secondary step action
Hold Step + Encoder = edit parameter lock on that step
REC + Step = live/lock recording behavior, depending on mode
```

Parameter locks are a core workflow concept. Holding a step and moving an encoder should store a step-specific value for that parameter.

## Encoder Workflow

The 8 encoders are contextual.

Default Performance mapping:

```text
E1-E7 = live macros
E8 = Master Morph when morphing is available
```

Track mode mapping examples:

| Track | Example encoder targets |
|---|---|
| Kick | Tune, decay, drive, pitch envelope, click, body, compressor, level |
| Bass | Tune, cutoff, resonance, decay, drive, envelope amount, accent, level |
| Acid Synth | Cutoff, resonance, envelope mod, decay, accent, slide, distortion, delay send |
| Sampler | Start, end, pitch, filter, decay, slice, retrig, level |

The display must always show the current encoder assignment.

## Morph Scene Workflow

Morph Scene is a mandatory performance feature.

A morph scene is a performative snapshot of selected parameter targets. Unlike a static preset, it can be blended in gradually using a morph amount.

Example scene purposes:

- Normal groove
- Acid build-up
- Distorted kick drop
- Filtered breakdown
- Delay feedback rise
- Reverb wash
- Full drop return

### Morph Scene Mode

```text
MODE → MORPH SCENE
```

In Morph Scene mode:

```text
Step buttons 1-16 = recall morph scenes 1-16
SHIFT + Step = save current state into that morph scene slot
E1-E7 = scene macro controls
E8 = Master Morph
```

### Master Morph

E8 is the preferred Master Morph control.

```text
0%   = original/current state
50%  = partial transition toward selected scene
100% = full selected scene state
```

This allows live transitions such as opening the Acid filter, increasing resonance, raising delay send and adding distortion without turning several controls at once.

## Pattern Workflow

Pattern mode uses the 16 step buttons as pattern slots.

```text
Pattern Mode
Step buttons 1-16 = select or queue pattern slots
SHIFT + Step = secondary pattern action
FUNC + Step = pattern management action
```

Pattern switching should support queued changes so the performer can prepare transitions without breaking timing.

## Project and System Workflow

Project and system operations are not primary live actions.

They can use:

- Display
- Touchscreen, if available
- FUNC + MODE menu access
- PAGE navigation
- BACK for cancellation

Examples:

- Project load/save
- Audio settings
- MIDI/sync settings
- Display and LED settings
- Storage management
- Diagnostics

## Safety Rules

Actions that can destroy data or interrupt a live set must require explicit interaction.

Protected actions:

- Delete project
- Clear pattern
- Overwrite kit or scene bank
- Format storage
- Factory reset
- Hard stop / kill FX tails

Recommended protection methods:

- Long press
- Confirmation screen
- SHIFT + button combo
- FUNC layer away from default performance actions
