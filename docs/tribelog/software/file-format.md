# File Format

This document describes the file structure for TribeLog sessions.

## Session Folder

Each session should use a dedicated folder.

Example:

```text
TRIBE-2026-06-16-001/
├─ audio.wav
├─ audio.flac
├─ metadata.json
└─ sync.log
```

## Audio Files

Initial targets:

- WAV for original recording
- FLAC for archive copy

## Metadata File

The metadata file should use JSON.

## Naming Goals

File names should be:

- Stable
- Human readable
- Easy to sort
- Easy to sync

## Notes

The final format should support both local browsing and automated upload workflows.
