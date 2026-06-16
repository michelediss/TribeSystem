# Screens

This document defines the planned TribeBox screen structure.

TribeBox uses a 7 inch display placed in the upper-left area of the device. The display is primarily a live feedback surface. Touch may be supported for setup and configuration, but live performance must remain possible through physical controls only.

## Screen Status

```text
Status: v0.1 closed
Display: 7 inch, upper-left position
Touch: optional setup/configuration only
Live control: physical controls first
```

## Screen Principles

- The screen must be readable while standing during a live set.
- The screen must always show what the 8 encoders currently control.
- The selected track must always be visible.
- Transport, BPM, pattern and morph scene state must remain visible in live modes.
- Warnings must be short, clear and visible.
- The screen should not become a small DAW.

## Performance Dashboard

The Performance Dashboard is the default boot screen and main live screen.

It shows:

- BPM
- Pattern
- Bar/step position
- Transport status
- Selected track
- Track activity and mute state
- Encoder assignments
- Active morph scene
- Master morph amount
- Critical warnings

Example layout:

```text
┌──────────────────────────────────────┐
│ 145.00 BPM | A03 | BAR 2/4 | RUN     │
├──────────────────────────────────────┤
│ 1 KICK   ███████   active            │
│ 2 BASS   █████     active            │
│ 3 PERC1  ███       mute              │
│ 4 PERC2  ██        active            │
│ 5 HAT    ██████    active            │
│ 6 PERC3  ████      active            │
│ 7 ACID   ████████  active            │
│ 8 SAMP   ███       active            │
├──────────────────────────────────────┤
│ E1 Drive   E2 Cutoff  E3 Reso  E4 Decay │
│ E5 Delay   E6 Reverb  E7 Crush E8 Morph │
├──────────────────────────────────────┤
│ Selected: ACID | Scene: 04 | Morph 67% │
└──────────────────────────────────────┘
```

## Encoder Assignment Area

Because the encoder bank is contextual, the screen must always show the current mapping.

Recommended format:

```text
E1 Tune      E2 Decay     E3 Drive     E4 Level
E5 Cutoff    E6 Reso      E7 Send      E8 Morph
```

This area should update immediately when:

- MODE changes
- PAGE changes
- Track changes
- Step is held
- Morph Scene mode is active
- FX or Mixer mode is active

## Track Screen

The Track screen shows parameters for the selected fixed track.

Fixed tracks:

1. Kick
2. Bass
3. Perc 1
4. Perc 2
5. Hat / Ride
6. Perc 3 / Noise / Utility
7. Acid Synth
8. Sampler

The screen should show:

- Track number and name
- Engine type
- Current parameter page
- Encoder assignments
- Step overview for the selected track
- Parameter lock indicators
- Mute/solo/activity state

Example Acid Synth page:

```text
┌──────────────────────────────────────┐
│ TRACK 7: ACID SYNTH | PAGE 1 SOUND   │
├──────────────────────────────────────┤
│ Cutoff 78 | Reso 64 | Env 52 | Decay 41 │
│ Accent 70 | Slide 22 | Drive 58 | Send 35 │
├──────────────────────────────────────┤
│ E1 Cutoff  E2 Reso  E3 Env  E4 Decay │
│ E5 Accent  E6 Slide E7 Drive E8 Send  │
├──────────────────────────────────────┤
│ Steps:  x . x . x x . . x . x . . x . │
└──────────────────────────────────────┘
```

## Step Edit Screen

The Step Edit screen appears when a step is held or when explicit step editing is active.

It should show:

- Selected track
- Selected step number
- Active trig state
- Parameter locks on that step
- Encoder assignment for lock editing

Workflow reminder:

```text
Hold Step + Encoder = edit parameter lock
SHIFT + Step = clear trig or secondary step action
```

Example:

```text
┌──────────────────────────────────────┐
│ STEP EDIT | TRACK 7 ACID | STEP 05   │
├──────────────────────────────────────┤
│ Trig: ON | Lock: Cutoff 92, Reso 80  │
├──────────────────────────────────────┤
│ E1 Cutoff  E2 Reso  E3 Env  E4 Decay │
│ E5 Accent  E6 Slide E7 Drive E8 Send │
└──────────────────────────────────────┘
```

## Mixer Screen

The Mixer screen gives a compact overview of all 8 tracks.

It should show:

- Track level meters
- Mute status
- Send levels
- Selected track
- Encoder assignment

Example:

```text
┌──────────────────────────────────────┐
│ MIXER | PAGE 1 LEVELS                │
├──────────────────────────────────────┤
│ 1 KICK ███████  2 BASS █████         │
│ 3 P1   ███      4 P2   ██            │
│ 5 HAT  ██████   6 P3   ████          │
│ 7 ACID ████████ 8 SAMP ███           │
├──────────────────────────────────────┤
│ E1 Kick E2 Bass E3 P1 E4 P2          │
│ E5 Hat  E6 P3   E7 Acid E8 Sampler   │
└──────────────────────────────────────┘
```

## FX Screen

The FX screen shows live effect parameters.

Possible parameters:

- Delay
- Reverb
- Distortion
- Compressor
- Filter
- Stutter
- Freeze
- Feedback

Example:

```text
┌──────────────────────────────────────┐
│ FX | PAGE 1 PERFORMANCE              │
├──────────────────────────────────────┤
│ Delay 35 | Reverb 18 | Drive 61      │
│ Crush 20 | Stutter OFF | Freeze OFF  │
├──────────────────────────────────────┤
│ E1 Delay  E2 Reverb E3 Drive E4 Crush│
│ E5 Filter E6 Stuttr E7 Freeze E8 Feed│
└──────────────────────────────────────┘
```

## Morph Scene Screen

Morph Scene is a mandatory performance screen.

A morph scene is a performative parameter snapshot that can be blended in gradually using a morph amount.

In Morph Scene mode:

```text
Step buttons 1-16 = recall morph scenes 1-16
SHIFT + Step = save current state into that morph scene slot
E1-E7 = scene macro controls
E8 = Master Morph
```

The screen must show:

- Active scene slot
- Scene name or number
- Master morph amount
- Parameters affected by the selected scene
- Encoder assignment
- Save/overwrite feedback when using SHIFT + Step

Example:

```text
┌──────────────────────────────────────┐
│ MORPH SCENE | SLOT 04 | ACID BUILD   │
├──────────────────────────────────────┤
│ Morph: 67%                           │
│ Targets: Acid cutoff, reso, drive,   │
│ delay send, kick drive, hat filter   │
├──────────────────────────────────────┤
│ E1 KickDrv E2 AcidCut E3 AcidRes E4 Delay │
│ E5 HatFilt E6 Reverb  E7 Crush   E8 Morph │
├──────────────────────────────────────┤
│ 01 02 03 [04] 05 06 07 08            │
│ 09 10 11  12  13 14 15 16            │
└──────────────────────────────────────┘
```

## Pattern Screen

The Pattern screen uses the 16 step buttons as pattern slots.

It should show:

- Current pattern
- Queued pattern
- Pattern bank
- Pattern length
- Active pattern slot grid
- Whether pattern switching is instant or queued

Example:

```text
┌──────────────────────────────────────┐
│ PATTERN | BANK A | CURRENT A03       │
├──────────────────────────────────────┤
│ Queued: A04 | Change: next bar       │
├──────────────────────────────────────┤
│ 01 02 [03] 04 05 06 07 08            │
│ 09 10  11  12 13 14 15 16            │
└──────────────────────────────────────┘
```

## Project / System Screens

Project and System screens are not primary live screens.

They may use touch interaction when available.

Project screen examples:

- Load project
- Save project
- Save as
- Pattern management
- Scene bank management
- Sample management

System screen examples:

- Audio settings
- MIDI / sync settings
- Display settings
- LED settings
- Storage status
- Diagnostics
- Network status
- Update tools

## Warning and Status Messages

Warnings should be concise and visually strong.

Important warning examples:

- Clipping
- CPU overload
- Sync lost
- Storage low
- Unsaved changes
- Recording active
- Pattern queued
- Scene saved
- Scene overwrite confirmation

Warnings should not hide the main performance state unless the action is critical.
