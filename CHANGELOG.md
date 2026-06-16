# Changelog

All notable changes to Tribe System will be documented in this file.

The format is inspired by Keep a Changelog.

---

## [0.3.0] - 2026-06-16

### Added

- Initial public repository documentation.
- `README.md` with the project overview.
- `ROADMAP.md` with the development phases.
- `CHANGELOG.md` for tracking project evolution.

### Defined

- Tribe System as an ecosystem composed of:
  - TribeBox
  - TribeVision
  - TribeLog
- Mackie Mix12FX as the reference performance mixer.
- OSC as the internal communication protocol between TribeBox and TribeVision.
- TribeLog as a geolocated audio recorder.

### TribeBox

- Defined as the main audio performance instrument.
- Planned 8-track architecture.
- Planned step sequencing.
- Planned independent track length.
- Planned Euclidean trigger engine.
- Planned generative pitch engine.
- Planned sample playback.
- Planned acid synth engine.
- Planned internal FX engine.
- Planned Fill and Retrigger features.
- Planned Parameter Locks and Trig Conditions.
- Defined default 4-output routing:
  - Out 1 = Kick
  - Out 2 = Bass / Rumble
  - Out 3 = Drums
  - Out 4 = Acid / Synth / FX

### TribeVision

- Defined as the generative visual engine.
- Planned Raspberry Pi 5 based architecture.
- Planned openFrameworks visual engine.
- Planned OSC input.
- Planned projection mapping layer.
- Planned smartphone mapping control.
- Planned dual HDMI output.

### TribeLog

- Defined as a Raspberry Pi based geolocated audio recorder.
- Planned stereo recording from mixer output.
- Planned WAV / FLAC recording.
- Planned GPS metadata.
- Planned timestamp metadata.
- Planned cloud sync.

### Hardware / Live Case

- Defined the all-in-one live case concept.
- Defined minimal external connections:
  - IEC Power In
  - HDMI Projector Out
  - Mackie Main Out L/R
- Defined patch panel concept.
- Defined centralized internal power distribution concept.

---

## [0.2.0]

### Changed

- Removed TribeFX as a separate Raspberry Pi based module.
- Integrated the FX Engine into TribeBox.
- Simplified the system architecture.

---

## [0.1.0]

### Added

- Initial concept of Tribe System.
- Initial concept of a modular live setup for Tekno, Tribe and Acid performance.
