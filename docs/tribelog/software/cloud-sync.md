# Cloud Sync

This document describes the planned cloud synchronization system for TribeLog.

## Goal

Upload completed session files to a remote storage service.

## Upload Targets

Possible targets:

- Private server
- Nextcloud
- S3 compatible storage
- WebDAV storage

## Upload Set

Each session upload should include:

- Audio file
- Metadata file
- Optional log file

## Sync Strategy

Recommended workflow:

1. Record locally.
2. Save metadata locally.
3. Verify files exist.
4. Upload when network is available.
5. Mark session as synced.

## Failure Handling

If upload fails, files must remain available locally for retry.

## Notes

Cloud sync should never block or interrupt active recording.
