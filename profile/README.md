# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible operating system built around portable Sable-owned applications, semantic contracts, and services with bounded Android/device adapters.

## Repository architecture

- `sableos` — project architecture, ADRs, roadmap, and validation history.
- `platform_manifest` — authoritative multi-repository source composition.
- `packages_apps_SableStart` — common Sable Start launcher and shell.
- `platform_sable` — common semantic contracts, services, and Android adapters.
- `vendor_sable` — common Android product integration.
- `device_sable_*` — bounded target-specific integration.
- `build` — host bootstrap, build orchestration, and validation tooling.

## Current reference target

Google Pixel 7 (`panther`) on GrapheneOS `2026081300` / Android 17 is the primary development and validation reference.

The architecture is intentionally multi-device: common Sable product behavior should not be forked merely because hardware or Android substrate changes.

## Project principle

Applications provide capabilities; Sable organizes people, attention, actions, privacy, and intent.

Runtime milestones close on evidence rather than compilation alone.
