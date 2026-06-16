# Recorder Interface

This document describes the minimal user interface for TribeLog.

## Goal

The recorder interface should be simple, reliable and usable during live setup.

## Physical Controls

Minimum controls:

- REC button
- Status LED
- OLED display

## REC Button

The REC button should control recording state.

Planned behavior:

- Press once to start recording
- Press again to stop recording
- Long press may be used for safe shutdown or secondary actions

## Status LED

The status LED should provide immediate feedback.

Example states:

- Off: idle or powered down
- Solid: ready
- Blinking: recording
- Fast blinking: error or storage issue

## OLED Display

The OLED display should show essential information only.

Suggested fields:

- Recording state
- Elapsed time
- Storage status
- GPS lock status
- Upload status

## Workflow

Basic live workflow:

1. Power on Tribe System.
2. Wait for TribeLog ready state.
3. Check GPS lock and storage status.
4. Press REC to start recording.
5. Press REC to stop recording.
6. Wait for file write and metadata save.

## Design Principle

TribeLog should not require complex menus during a live session.
