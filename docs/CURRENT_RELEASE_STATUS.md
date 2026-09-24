# SableOS current development status

Status date: **2026-09-24**

## Executive state

Pixel 7 / Panther R9 physical acceptance is complete. Panther is now a frozen
touch-first reference / regression target rather than the active feature target.

```text
R9_PANTHER_PHYSICAL_ACCEPTANCE=PASS
R9_PANTHER_DEVELOPMENT=HOLD_REFERENCE_MAINTENANCE_ONLY
ACTIVE_PRODUCT_DIRECTION=KEYBOARD_FIRST
ACTIVE_DEVICE_1=TITAN2
ACTIVE_DEVICE_2=TITAN2_ELITE
FUTURE_DEVICE=Q27
LOCAL_DIRECT_CI=ACTIVE
PRODUCTION_SIGNING=DEFERRED
```

The accepted Panther image proved standalone SableLauncher HOME, Quickstep
Recents-only, the current first-party application composition, Settings-hosted
global appearance, Network Manager, SableOS developer-notification identity,
Reader/Text Reader separation, Phone/People alphabet navigation and physical
Light/Dark propagation.

## Active device roles

| Device | Role | Current state |
| --- | --- | --- |
| Pixel 7 / panther | frozen reference | R9 physical acceptance PASS; maintenance/regression only |
| Unihertz Titan 2 | PORTABILITY / N0 | active keyboard-first target; stock/camera research underway |
| Unihertz Titan 2 Elite | PORTABILITY candidate / N0 | independent physical baseline starts when device is available |
| Zinwa Q27 | RESEARCH / future product candidate | deferred until shipped hardware/firmware qualifies |
| Pixel 4a 5G / bramble | historical reference | frozen |

Titan 2 and Titan 2 Elite share common keyboard-first product semantics but are
separate hardware qualification targets.

## Current interaction architecture

SableOS uses one product core with multiple interaction profiles:

```text
touch-first
    Panther reference

keyboard-first
    Titan 2
    Titan 2 Elite
    future Q27
```

Common application/service semantics should not fork because a device has a
physical keyboard, square display, different SoC or different vendor BSP.

Keyboard-first work adds deterministic focus, type-to-search, shortcut/command
navigation, square/near-square responsive layouts and hardware-key adapters
while preserving touch as a secondary path.

## Launcher architecture

The accepted R9 product architecture is:

```text
org.sableos.launcher / SableLauncher
    user-facing HOME / Start / All Apps / Search / Peek / app context

Launcher3QuickStep
    retained privately for Overview / Recents / task/gesture substrate
    not HOME-eligible
```

Historical public documents that describe Launcher3 as the user-facing HOME
owner are superseded by this status.

## Multi-device build/deployment contract

The canonical private integration entry point is device/release neutral:

```text
build/sable.sh <device> <release> <function> [options]
```

Known canonical device IDs:

```text
panther
titan2
titan2-elite
q27
```

A physical serial is intentionally not part of build/image identity. It is
required only for device-contact operations such as flash and runtime
acceptance. Device adapters remain fail-closed until their build/flash contract
is physically qualified.

The next tooling work is to generalize artifact descriptors beyond Panther
target-files and split deployment into common safety/evidence policy plus
device-specific flash transport/partition logic.

## Keyboard-first system applications

Two capabilities move onto the active product path:

**Sable Camera**
- common Camera2/vendor-HAL based camera application;
- square/near-square keyboard-first UI;
- device capability profiles;
- optional SYSTEM_CAMERA privilege only where physical evidence proves it is
  necessary and safe.

**Sable Keyboard / input**
- offline-capable IME included in keyboard-first system images;
- common text composition separated from device-specific keylayout/keycharacter,
  Fn/Sym, shortcut, backlight and pointer behavior.

Titan 2 camera research already supports a capability-driven camera-core /
device-profile direction. Titan 2 Elite requires its own independent evidence.

## Non-Pixel support levels

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

A booting GSI proves neither N1 nor N2. Kernel/vendor/firmware/AVB/recovery/
telephony/security lifecycle remain independent support gates.

## Repository ownership

Current transition policy:

- `sableos-project` is the target canonical public home for reusable Sable
  source, architecture and build contracts;
- the private integration repository remains the integration/release authority
  while source is being decomposed and publication-reviewed;
- do **not** publish the private monorepo wholesale;
- migrate components individually after provenance, licensing, secret/private
  path review and independent build/test closure;
- raw firmware, device serials, private evidence and unreviewed vendor
  diagnostics stay outside public Git.

See `SOURCE_OWNERSHIP_AND_PUBLICATION.md`.

## Execution order

```text
Panther frozen reference
  -> public/private documentation reconciliation
  -> multi-device artifact + deployment abstraction
  -> Titan 2 keyboard-first N0 qualification
  -> Titan 2 Elite independent qualification
  -> Sable Camera + Sable Keyboard system integration
  -> Q27 after shipped-hardware acceptance
  -> production signing/OTA before public release
```

Historical R5-R9 documents remain evidence records. This file is the current
organization-level execution authority.
