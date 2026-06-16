# Metadata

This document describes the metadata stored by TribeLog for each session.

## Goal

Each recording should be paired with a machine-readable metadata file.

## Metadata Fields

Planned fields:

- Session ID
- Start timestamp
- End timestamp
- Duration
- GPS latitude
- GPS longitude
- Audio format
- Sample rate
- Bit depth
- Notes
- Tags

## Session ID

Example format:

```text
TRIBE-YYYY-MM-DD-001
```

## Metadata File

The initial target format is JSON.

## Notes

Metadata should be saved locally together with the audio file before any cloud sync is attempted.
