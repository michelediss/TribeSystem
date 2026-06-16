# Controls Layout

This document defines the planned physical control layout for TribeBox.

TribeBox is designed as a compact live groovebox for Tekno, Tribe, Hardtek and Acid performances. The control surface must support live performance without requiring touchscreen interaction.

## Layout Status

```text
Status: v0.1 closed
Priority: compact live performance
Reference direction: Elektron-style groovebox workflow
```

## Physical Layout

The physical layout is divided into three main areas:

```text
┌──────────────────────────────────────────────┐
│ ┌──────────────────────┐  E1   E2   E3   E4  │
│ │                      │                      │
│ │      DISPLAY 7"      │  E5   E6   E7   E8  │
│ │    optional touch    │                      │
│ └──────────────────────┘  encoder 2x4 grid   │
├──────────────────────────────────────────────┤
│ [01] [02] [03] [04] [05] [06] [07] [08]      │
│ [09] [10] [11] [12] [13] [14] [15] [16]      │
│        16 RGB step buttons, 2x8 layout        │
├──────────────────────────────────────────────┤
│ PLAY   STOP   REC   SHIFT   FUNC   MODE PAGE BACK │
└──────────────────────────────────────────────┘
```

## Display

The display is placed in the upper-left area of the device.

Planned display:

- 7 inch display
- Touchscreen optional
- Touch is intended for setup and configuration only
- Live performance must remain possible using physical controls only

The display is mainly used for:

- Live feedback
- Encoder mapping
- Track status
- Pattern status
- Morph scene status
- Warnings and system feedback
- Setup and configuration when needed

## Encoders

TribeBox uses 8 endless push encoders arranged in a 2x4 grid to the right of the display.

```text
E1  E2  E3  E4
E5  E6  E7  E8
```

Each encoder should provide visual feedback:

- RGB LED ring or equivalent feedback
- Push/click interaction
- Context-aware parameter assignment

The encoder bank changes depending on the active mode.

Example assignments:

| Mode | Encoder role |
|---|---|
| Performance | 8 live macro controls |
| Track | Parameters of the selected track |
| Mixer | Level, send, pan or track mix parameters |
| FX | Effect parameters |
| Step Edit | Parameter locks for the selected step |
| Morph Scene | Scene macro controls and master morph |

E8 should be reserved as the preferred **Master Morph** control when Morph Scene mode is active.

## Step Buttons

TribeBox uses 16 RGB step buttons arranged as a compact 2x8 grid.

```text
01  02  03  04  05  06  07  08
09  10  11  12  13  14  15  16
```

The step buttons are multi-role controls. There are no dedicated track buttons in the current layout.

| Mode | Step button role |
|---|---|
| Performance | Step triggers for the selected track |
| Track Select | Buttons 1-8 select the 8 fixed tracks |
| Mute | Buttons 1-8 mute/unmute the 8 fixed tracks |
| Pattern | Buttons 1-16 select pattern slots |
| Morph Scene | Buttons 1-16 recall morph scenes |
| Step Edit | Hold step + encoder edits parameter locks |

All step buttons should provide RGB feedback for state, mode and activity.

## Global Buttons

The global button set is fixed at 8 buttons:

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

### Button Roles

| Button | Primary role |
|---|---|
| PLAY | Start transport |
| STOP | Stop transport |
| REC | Recording, live recording, parameter lock recording |
| SHIFT | Momentary modifier layer |
| FUNC | Secondary function layer and menu access |
| MODE | Cycle or open performance modes |
| PAGE | Change page inside the current mode |
| BACK | Go back, cancel, exit current screen |

### Safety Shortcuts

Destructive or dangerous actions should not be one-click actions.

Recommended interactions:

| Action | Interaction |
|---|---|
| Panic / Stop all | Long press STOP |
| Hard stop / kill tails | SHIFT + STOP |
| Clear trig | SHIFT + Step |
| Save scene | SHIFT + Step in Morph Scene mode |
| Open main menu | FUNC + MODE |
| Previous page | SHIFT + PAGE |

## Fixed Track Layout

TribeBox uses 8 fixed tracks.

1. Kick
2. Bass
3. Perc 1
4. Perc 2
5. Hat / Ride
6. Perc 3 / Noise / Utility
7. Acid Synth
8. Sampler

The fixed layout is intentional. It favors live speed and muscle memory over fully abstract track assignment.

## Design Principles

- Compact groovebox form factor
- Physical controls first
- Display as feedback, not as the main live input method
- Touchscreen allowed for setup only
- RGB feedback for encoders and buttons
- No dedicated track buttons in v0.1
- 16 step buttons handle track selection, mute, pattern and scene roles through modes
