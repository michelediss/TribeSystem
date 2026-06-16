# Tribe System Roadmap

This roadmap tracks the planned development phases for Tribe System.

The project is currently in the documentation and design phase.

---

## Phase 0 — Concept Definition

Status: Completed

Goals:

- Define the overall Tribe System architecture
- Define the three main modules: TribeBox, TribeVision and TribeLog
- Define the role of the Mackie Mix12FX mixer
- Define the live case concept
- Define the internal communication protocol

Main decisions:

- TribeBox handles sequencing, synthesis, sampling and FX
- TribeVision handles visuals and projection mapping
- TribeLog handles geolocated audio recording
- OSC is used for TribeBox → TribeVision communication
- MIDI is optional for future external hardware support

---

## Phase 1 — Documentation Structure

Status: In progress

Goals:

- Create the repository structure
- Write the first technical specifications
- Document all major modules separately
- Create hardware and software documentation areas

Planned documents:

- Tribe System overview
- TribeBox specification
- TribeVision specification
- TribeLog specification
- Audio routing
- OSC protocol
- Power architecture
- Live case architecture
- Patch panel architecture

---

## Phase 2 — TribeBox Prototype

Status: Planned

Goals:

- Set up Raspberry Pi 5
- Evaluate Zynthian as the audio base
- Test audio interface options
- Build a first 4-output audio routing prototype
- Implement or prototype the sequencer core

Key features to prototype:

- 8-track structure
- Step sequencer
- Independent track length
- Euclidean trigger engine
- Fill
- Retrigger
- Root note system
- Basic sample playback
- Basic acid synth
- Basic OSC output

---

## Phase 3 — TribeVision Prototype

Status: Planned

Goals:

- Set up Raspberry Pi 5 16 GB
- Set up openFrameworks
- Receive OSC events from TribeBox or a test sender
- Generate first MIDI/OSC-reactive visuals
- Test dual HDMI output
- Prototype mapping layer
- Prototype smartphone mapping control

Key features to prototype:

- Visual engine
- OSC listener
- Visual DNA concept
- Projection mapping
- Smartphone web UI
- Projector + 7" monitor workflow

---

## Phase 4 — TribeLog Prototype

Status: Planned

Goals:

- Set up Raspberry Pi based recorder
- Test stereo line input from mixer
- Integrate GPS module
- Generate session metadata
- Save WAV or FLAC recordings
- Test local backup
- Test cloud sync

Key features to prototype:

- REC button
- Status LED
- OLED status screen
- GPS timestamp
- GPS coordinates
- Metadata JSON
- Nextcloud / WebDAV sync

---

## Phase 5 — Integrated System Prototype

Status: Planned

Goals:

- Connect TribeBox, TribeVision and TribeLog in one system
- Test internal networking
- Test OSC communication
- Test audio routing through Mackie Mix12FX
- Test external HDMI output
- Test power distribution

Target live connections:

```text
IEC Power In
HDMI Projector Out
Mackie Main Out L/R
```

---

## Phase 6 — Live Case Prototype

Status: Planned

Goals:

- Select flight case dimensions
- Design internal mounting structure
- Design patch panel
- Mount Mackie Mix12FX
- Mount TribeBox controls
- Mount TribeVision monitor
- Mount TribeLog controls
- Install power distribution
- Install cooling

Key parts:

- Removable internal frame
- Patch panel
- IEC input
- HDMI output
- Ethernet service output
- Internal 12V power distribution
- Dedicated buck converters for Raspberry modules

---

## Phase 7 — Field Testing

Status: Planned

Goals:

- Test full live workflow
- Test thermal stability
- Test long running sessions
- Test recording reliability
- Test visual mapping in different spaces
- Test setup and teardown time

Success criteria:

- The system can boot reliably
- The system can run for several hours
- Audio remains stable
- Visuals remain stable
- Recordings are saved correctly
- Projector mapping can be adjusted quickly

---

## Phase 8 — Tribe System v1.0

Status: Future

Goals:

- Stable hardware layout
- Stable software architecture
- Public technical documentation
- Reproducible build process
- Open-source release

Potential v1.0 deliverables:

- Full documentation
- Software repositories
- Wiring diagrams
- Patch panel drawings
- CAD files
- Example projects
- Example visual presets
