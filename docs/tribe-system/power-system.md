# Power System

This document describes the planned power architecture.

## External Input

The case should expose one IEC power input.

## Internal Distribution

A central internal power section should feed:

- TribeBox
- TribeVision
- TribeLog
- auxiliary displays
- internal network devices

The Mackie mixer may keep its original power supply inside the case.

## Recommended Direction

Use a 12V internal supply with dedicated step-down converters for each Raspberry Pi module.

Each module should have its own protected power branch.
