# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible operating system built around portable Sable-owned applications, semantic contracts, and services with bounded Android/device adapters.

## Current product objective

The immediate objective is a dependable daily-driver phone on the current Pixel 7 (`panther`) reference target before broad visual polish or replacement of mature Android applications.

The active development train is:

```text
R5  migrated-source build/reconstruction closure
 |
 v
R6  real Sable Start launcher + local-time greeting
 |
 v
R7  daily-driver phone validation + explicit default-app decisions
 |
 v
R8  shared Sable design/theme/customization foundation
 |
 v
R9  first native Sable utilities, beginning with Calculator
 |
 v
R10+ deliberate replacement/expansion based on documented value
```

Start future implementation sessions with [`docs/REQUIREMENTS_INDEX.md`](../docs/REQUIREMENTS_INDEX.md). It points to the owning requirements for each milestone and defines the read-before-code protocol.

The organization-wide normative plan is in [`docs/DEVELOPMENT_RELEASE_PLAN.md`](../docs/DEVELOPMENT_RELEASE_PLAN.md). The default-application/replacement decision framework is in [`docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md`](../docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md).

These `R*` labels are development milestones, not public semantic SableOS versions. Exact product/build identity remains revision- and manifest-based.

## Repository architecture

- `sableos` — intended future home for project architecture, ADRs, roadmap, and validation history after the existing repository transition is complete.
- `.github` — current organization-wide policy and development-plan documentation while the central `sableos` repository transition is incomplete.
- `platform_manifest` — authoritative multi-repository source composition.
- `packages_apps_SableStart` — common Sable Start launcher and shell.
- `platform_sable` — common semantic contracts, services, Android adapters, and shared Sable product/design contracts.
- `vendor_sable` — common Android product integration and validated default-package composition.
- `device_sable_*` — bounded target-specific integration and device qualification.
- `build` — host bootstrap, build orchestration, reconstruction, and validation tooling.

## Current reference target

Google Pixel 7 (`panther`) on the validated GrapheneOS `2026081300` / Android 17 substrate work is the primary development and validation reference.

The architecture is intentionally multi-device: common Sable product behavior should not be forked merely because hardware or Android substrate changes.

## Daily-driver first

The near-term baseline is practical phone functionality:

- calls;
- contacts sufficient for call/message workflows;
- SMS/MMS;
- Wi-Fi and cellular data;
- Internet/browser access;
- notifications;
- Settings;
- camera/photos/files;
- clock/alarm;
- calculator;
- complete launcher-visible application discovery and launch.

SableOS does **not** need to replace every Android application to reach this baseline. Complex applications such as Phone, Messaging, Browser, and Camera should initially use a proven implementation unless a documented replacement case is approved. Sable Calculator is the first intended low-privilege native Sable utility.

## Project principle

Applications provide capabilities; Sable organizes people, attention, actions, privacy, and intent.

Runtime milestones close on evidence rather than compilation alone. If an implementation decision is not covered by the current requirements, update the requirements before inventing new product behavior.