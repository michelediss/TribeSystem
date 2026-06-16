# Tribe System

Tribe System is an open-source hardware/software ecosystem for live Tekno, Tribe, Hardtek and Acid performances.

The project is designed as a portable live system that combines sound generation, visual generation and geolocated recording inside a modular DIY architecture.

## Core Modules

```text
Tribe System
├─ TribeBox
├─ TribeVision
└─ TribeLog
```

## TribeBox

TribeBox is the main audio performance instrument.

It is designed as a Raspberry Pi based groovebox focused on live Tribe/Tekno workflows.

Planned features:

- 8-track sequencer
- Step sequencing
- Independent track length
- Euclidean trigger engine
- Scale lock / root note system
- Generative pitch engine
- Sample playback
- Acid synth engine
- Bass / rumble engine
- Internal FX engine
- Fill and retrigger
- Parameter locks
- OSC output to TribeVision

Default audio routing:

```text
Out 1 = Kick
Out 2 = Bass / Rumble
Out 3 = Drums
Out 4 = Acid / Synth / FX
```

## TribeVision

TribeVision is the visual engine of the system.

It is based on Raspberry Pi and openFrameworks and receives OSC events from TribeBox.

Planned features:

- Generative visuals
- Video layers
- OSC input
- Visual DNA presets
- Projection mapping
- Smartphone-based mapping control
- Dual HDMI output
- Projector output + 7" monitor output

## TribeLog

TribeLog is a geolocated audio recorder for documenting live sessions.

It is based on Raspberry Pi and records the stereo output of the mixer while storing metadata for each session.

Planned features:

- Stereo recording from mixer line output
- WAV / FLAC recording
- GPS coordinates
- GPS timestamp
- Session metadata
- Local backup
- Automatic cloud sync
- Minimal physical UI

## Mixer

The current reference mixer is:

```text
Mackie Mix12FX
```

The mixer is treated as a physical performance surface for:

- Gain staging
- EQ
- Muting
- Level balancing
- Master output

## Communication

Internal communication between TribeBox and TribeVision uses OSC.

MIDI is not planned as the internal protocol, but may be supported for external gear in the future.

Example OSC events:

```text
/transport/start
/transport/stop
/clock/bpm
/track/1/trigger
/track/7/cutoff
/fx/delay
/macro/8
/visual/palette
```

## Repository Status

This repository is currently in the concept and technical documentation phase.

No stable hardware or software release is available yet.

## Planned Repository Areas

```text
docs/
software/
electronics/
cad/
assets/
experiments/
```

## License

License to be defined.
