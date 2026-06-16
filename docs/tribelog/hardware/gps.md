# GPS

## Purpose

The GPS module provides location and time metadata for each recording session.

## Stored Data

Planned metadata:

- Latitude
- Longitude
- Altitude if available
- GPS timestamp
- Fix status

## Usage

GPS data is stored in the session metadata file.

The audio file does not need to contain GPS data directly in the first prototype.

## Design Goals

- Reliable position fix
- Clear status indication
- Simple metadata format

## Notes

If GPS fix is not available, the recording should still work and the metadata should mark the missing position.
