# SableOS current development status

Status date: **2026-09-24**

This file is the organization-level current-status authority. Historical R3-R9
documents remain evidence records and must not be read as current execution
state when they conflict with this file.

## Executive state

```text
Pixel 7 / Panther        R9 PHYSICAL ACCEPTANCE PASS / REFERENCE_FROZEN
K1 artifact foundation  PASS / MERGED
K2 deployment boundary  PASS / MERGED
Titan 2                  ACTIVE KEYBOARD-FIRST PORTABILITY / N0 RESEARCH
Titan 2 Elite            NEXT INDEPENDENT KEYBOARD-FIRST PORTABILITY TARGET
Q27                      RESEARCH / FUTURE PRODUCT CANDIDATE
Production signing       DEFERRED
```

The accepted Panther image remains bound to its exact physically qualified
source and artifact identity. Later documentation/tooling commits do not become
new Panther image qualification sources.

## Exact accepted / tooling identities

```text
R9_PANTHER_IMAGE_SOURCE=6f1d6d2f0f2525067874238c4b797ad58f2bcbc6
R9_PANTHER_TARGET_FILES_SHA256=08ef429c7f9eef17de7ddad4ce9baa81911950e8d34b6751b6e4588f66821502
R9_PANTHER_REFERENCE_BRANCH=reference/panther-r9-accepted-20260924

K1_K2_FULL_CI_SOURCE=dec78f0f6a86c0236cd4783b14efb7ee86e45215
K1_K2_PRIVATE_MERGE=f11502202e8913bdb2e7317d824d26577a98e7a1
K1_K2_LOCAL_FULL_CI=PASS
```

Later public documentation commits do not change these image/tooling qualification
identities.

## Accepted Panther architecture

```text
org.sableos.launcher / SableLauncher
    HOME / Start / All Apps / Search / Peek / app context

Launcher3QuickStep
    retained privately for Recents / Overview / task/gesture substrate
    not HOME eligible
```

The standalone `org.sableos.start` runtime product is retired. Historical
SableStart repositories/documents remain presentation/history references.

The accepted first-party composition includes SableLauncher plus the qualified
Sable Calculator, Sudoku, Minesweeper, 2048, Media, Reader, Text Reader,
Hub/Messages, Mail, Weather and Calendar product set. Reader and Text Reader are
separate products: publication/Readium capability belongs to Sable Reader;
TXT/share/process-text/TTS/audio/OCR belongs to Sable Text Reader.

Global appearance authority is Settings. Follow-system/Light/Dark propagation
was physically confirmed on Panther. Vanadium is the browser reference.
Camera remains the documented upstream/preprocessed dark-presentation exception
for the frozen Panther R9 image.

## K1/K2 multi-device foundation

K1/K2 is merged in the private integration baseline.

Artifact registry v2 supports:

```text
target-files
full-device-images
gsi-system-image
system-product-bundle
boot-recovery-bundle
```

Artifact identity is based on device + release + source + exact artifact hashes.
A physical serial is never part of image identity.

The deployment architecture is now split:

```text
common deployment policy
    authorization
    artifact verification
    exact selected-serial binding
    evidence sealing
    preserved-data baseline where supported
    boot/credential-encrypted-data return
    callback dispatch

device adapter
    artifact kind
    partition/slot model
    flash transport
    AVB/vbmeta requirements
    restore strategy
    target-specific post-boot acceptance
```

Panther is the qualified `target-files` / A-B fastboot adapter. Titan 2,
Titan 2 Elite and Q27 remain fail-closed for release artifact registration,
flash planning and flashing until their own contracts are proven.

## Active direction

Active SableOS product work is now keyboard-first design plus parallel Titan
family research.

The keyboard-first profile is common product architecture, not a per-device app
fork. Required primitives include deterministic visible focus, arrow/D-pad
movement, Enter/Space activation, Back/Escape, type-to-search, shortcut/command
navigation, stable focus restoration, no focus traps and touch as a secondary
path.

Two system-image capabilities are explicit workstreams:

- **Sable Camera** — common Camera2/capability architecture, stock vendor
  HAL/ISP initially, device profiles below the common core, SYSTEM_CAMERA only
  where physical evidence proves value and negative-access tests pass.
- **Sable Keyboard / input** — offline-capable common IME/text composition
  separated from device-specific keylayout/keycharacter/Fn/Sym/backlight/
  pointer behavior.

## Device roles

| Device | Role | Assurance | Current state |
| --- | --- | --- | --- |
| Pixel 7 / panther | REFERENCE_FROZEN | accepted R9 reference | maintenance/regression only |
| Titan 2 | PORTABILITY | N0 active | keyboard/display/camera/restore research in parallel |
| Titan 2 Elite | PORTABILITY candidate | N0 pending | independent baseline required on retail hardware |
| Q27 | RESEARCH | unqualified | future candidate after shipped-hardware evidence |
| Pixel 4a 5G / bramble | historical reference | frozen | no active investment |

No new PRIMARY device is declared by the transition.

## Repository ownership

`sableos-project` is the target canonical public home for reusable Sable
source, architecture and build contracts.

The private integration repository remains the integration/release authority
during decomposition and publication review. Do not publish that monorepo
wholesale. Components migrate only after provenance, licensing, secret/private
path review, independent build/test instructions and parity are closed.

## Current execution order

```text
K1/K2 merged
  -> organization documentation reconciliation
  -> keyboard-first SableOS design
  -> Titan 2 adapter-input research / N0 bring-up planning
  -> Titan 2 first controlled Sable artifact + deployment qualification
  -> Titan 2 Elite independent sequence
  -> Q27 only after shipped hardware qualifies
  -> production signing / OTA release engineering later
```
