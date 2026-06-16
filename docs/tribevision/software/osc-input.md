# OSC Input

TribeVision receives control and performance data from TribeBox through OSC.

## Transport

- Protocol: OSC over UDP
- Sender: TribeBox
- Receiver: TribeVision
- Network: local internal network

## Message Types

Planned categories:

- Transport events
- Clock values
- Track triggers
- Track parameters
- FX values
- Macro values
- Visual preset data

## Purpose

OSC allows TribeVision to react to musical and performance events without depending on MIDI.

## Design Goals

- Human readable messages
- Easy debugging
- Extensible message structure
- Low implementation complexity

## Notes

The final OSC message reference will live in a shared protocol document.
