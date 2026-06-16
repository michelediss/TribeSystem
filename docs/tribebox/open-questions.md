# TribeBox Open Questions

This document tracks TribeBox design decisions that are still open.

Closed topics are intentionally not listed here anymore. Finalized decisions are documented in their dedicated files, such as UI layout/workflow, Pattern System, FX Engine, Sample Engine, Hybrid Sound Engine, and Sequencer Engine.

---

## 1. Mixer Engine — **OPEN**

Priority: **high**

The FX Engine defines processing and send effects, but the dedicated mixer behavior still needs to be specified.

Open questions:

- Track volume model
- Track gain staging
- Mute workflow
- Solo workflow, if any
- Performance mute behavior
- Track grouping, if any
- Delay send control per track
- Reverb send control per track
- Master level behavior
- Metering on the display
- Safety limiter interaction with master level
- Scene and Morph Scene interaction with mixer values

Related future documents:

- `docs/tribebox/audio/mixer-engine.md`
- `docs/tribebox/audio/gain-staging.md`

---

## 2. Project / File Format — **OPEN**

Priority: **high**

Several project-level concepts are already defined separately, but the complete project structure still needs a single canonical format.

Open questions:

- Project folder layout
- Project metadata file
- Pattern storage format
- Bank storage format
- Saved Chain storage format
- Morph Scene storage format
- Kit storage format
- Sound Preset storage format
- Track Sound storage format
- Pattern Override storage format
- Sample reference strategy
- `sample-index.json` placement and schema
- Sidecar `.tribe.json` interaction with project-level metadata
- Missing sample handling
- Autosave behavior
- Backup / snapshot behavior
- Versioning and migration strategy
- Import/export workflow

Related future documents:

- `docs/tribebox/software/project-format.md`
- `docs/tribebox/software/file-format.md`

---

## 3. Kit / Sound Library — **OPEN**

Priority: **medium-high**

The Sound Engine defines the conceptual structure: Sound Preset, Track Sound, Kit, Pattern Override, and Morph Scene. The library and workflow around these objects still need to be specified.

Open questions:

- Kit save workflow
- Kit load workflow
- Sound Preset save workflow
- Sound Preset load workflow
- Track Sound duplication
- Track Sound reset/default behavior
- Pattern Override visibility and edit workflow
- Factory library vs user library
- Project-local sounds vs global sounds
- Default kit behavior for new projects
- Naming conventions
- Folder structure for presets and kits
- Browser workflow for sounds and kits

Related future documents:

- `docs/tribebox/audio/kit-library.md`
- `docs/tribebox/audio/sound-presets.md`

---

## 4. MIDI / Sync / OSC — **OPEN**

Priority: **medium-high**

OSC output is already planned at a high level, but the full external sync and communication strategy is still open.

Open questions:

- MIDI clock input
- MIDI clock output
- MIDI start/stop/continue behavior
- USB MIDI support
- DIN MIDI support, if any
- Ableton Link support, if any
- OSC message structure
- OSC event categories
- OSC timing information
- OSC performance data exposure
- External controller mapping
- Latency compensation
- Sync error handling
- Multi-device synchronization roadmap

Related future documents:

- `docs/tribebox/software/midi-sync.md`
- `docs/tribebox/software/osc-output.md`
- `docs/tribebox/software/controller-mapping.md`

---

## 5. Audio I/O / Hardware Assumptions — **OPEN**

Priority: **medium-high**

TribeBox is now defined as a mono, live, sound-system-oriented device. The hardware and I/O assumptions need to reflect that clearly.

Open questions:

- Mono master output format
- Number of physical audio outputs
- Headphone output requirement
- Audio input requirement
- Internal resampling requirement
- USB audio support, if any
- Target sample rate
- Target buffer size
- Maximum acceptable latency
- Audio interface selection
- Output level target
- Ground/noise considerations for live rigs
- Hardware monitoring needs

Related future documents:

- `docs/tribebox/hardware/audio-io.md`
- `docs/tribebox/hardware/audio-interface.md`

---

## 6. Performance Macros — **OPEN**

Priority: **medium**

Morph Scenes are defined as a core performance concept, but macro assignment and encoder behavior still need a detailed specification.

Open questions:

- Global macro model
- Track macro model
- Mode-specific encoder mappings
- `E1`–`E8` behavior per mode
- Macro assignment workflow
- Macro range limits
- Morph Scene target assignment
- Morph Scene interpolation rules
- Safe reset behavior
- Panic / emergency clean state behavior
- Visual feedback rules for macro changes
- RGB encoder feedback behavior

Related future documents:

- `docs/tribebox/ui/performance-macros.md`
- `docs/tribebox/ui/morph-scene-workflow.md`

---

## 7. Hardware BOM — **OPEN**

Priority: **medium**

Some hardware assumptions are already emerging from UI and audio decisions, but the final BOM is still open.

Open questions:

- Final display selection
- Final audio interface selection
- Encoder model
- Button model
- RGB feedback hardware
- Storage selection
- Cooling strategy
- Power supply strategy
- Enclosure constraints
- Internal USB / GPIO / I2C / SPI layout
- Serviceability
- Ruggedness for live use

Related future documents:

- `docs/tribebox/hardware/bom.md`
- `docs/tribebox/hardware/enclosure.md`
- `docs/tribebox/hardware/control-surface.md`

---

## 8. Development Roadmap v0.1 — **OPEN**

Priority: **medium**

Once the main specifications are closed, TribeBox needs a concrete development roadmap that separates the desktop prototype, hardware prototype, and v0.1 live-ready target.

Open questions:

- MVP scope
- First Ubuntu desktop prototype
- First audio engine prototype
- First sequencer prototype
- First UI prototype
- Test sample library
- Hardware target for first prototype
- v0.1 feature freeze
- Future feature backlog
- Testing strategy
- Live rehearsal validation

Related future documents:

- `docs/tribebox/software/development-roadmap.md`
- `docs/tribebox/software/testing-strategy.md`

---

## Suggested next topic

Recommended next open question to close:

**Mixer Engine**

Reason: it is the next central layer connecting Track Sounds, FX sends, Morph Scenes, performance mute behavior, master level, metering, and the final safety limiter.
