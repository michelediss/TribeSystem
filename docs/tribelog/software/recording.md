# Recording

This document describes the recording engine of TribeLog.

## Goal

Capture the stereo output of the mixer as a reliable session recording.

## Input Source

The recording source is the mixer line output.

Possible sources:

- Tape output
- Control room output
- Dedicated stereo output

## Recording Control

Basic controls:

- Start recording
- Stop recording
- Show recording status
- Save session files

## Recording Format

Initial target:

- WAV for raw recording
- FLAC for compressed archive

## Reliability Goals

- Avoid data loss during long sessions
- Save metadata with each session
- Keep local backup before cloud sync

## Notes

Recording should remain independent from TribeBox and TribeVision so that it can continue even if other modules are restarted.
