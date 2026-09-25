# SableOS current development status

Status date: **2026-09-25**

This file is the organization-level current-status authority. Historical R3-R10
documents remain evidence records and must not be read as current execution
state when they conflict with this file.

## Executive state

```text
Pixel 7 / Panther        R9 HUB V1 PHYSICAL ACCEPTANCE PASS / REFERENCE_FROZEN
R9 Hub V1 closure        MERGED / PR #110
Keyboard-first design    MERGED / PR #108
K1/K2 foundation         MERGED
Titan 2                  ACTIVE KEYBOARD-FIRST PORTABILITY / N0 RESEARCH
Titan 2 Elite            NEXT INDEPENDENT KEYBOARD-FIRST PORTABILITY TARGET
Q27                      RESEARCH / FUTURE PRODUCT CANDIDATE
Production signing       DEFERRED
```

The accepted Panther image remains bound to its exact physically qualified
source and artifact identity. Later documentation/tooling commits do not become
new Panther image qualification sources.

## Exact accepted identities

```text
PRIVATE_INTEGRATION_MAIN=2624e1af
R10_KEYBOARD_FIRST_DESIGN_SOURCE=20da2daa
R10_KEYBOARD_FIRST_DESIGN_MERGE=2624e1af
R9_PANTHER_HUB_V1_MERGE=f175b00f
R9_PANTHER_IMAGE_SOURCE=edf62e5bb08372a1395841d6cc5d78d3148a7695
R9_PANTHER_TARGET_FILES_SHA256=a0b359613c4f30e9a834fba212e0b044a97d63ed0537c59471c31b99b627d285
R9_PANTHER_STANDARD_PRESERVED_DATA_FLASH=PASS
R9_PANTHER_PHYSICAL_ACCEPTANCE=PASS_WITH_PRESERVED_PLAY_STATE
```

The previous `6f1d6d2f` / `08ef...` Panther acceptance image remains historical
evidence. It is superseded as the current R9 reference by the final Hub V1
closure source `edf62e5b` and target-files hash above.

## Accepted Panther architecture

Panther is the frozen touch-first reference. It proves the common application
family, Sable Hub V1 and the canonical build/deploy evidence chain. It is no
longer the active feature-design target.

Sable Hub V1 is the accepted communications model:

```text
Priority | Messages | Email | People
```

Hub is an aggregator and interaction surface. Sable Mail owns mail accounts,
protocols, MIME/storage/security behavior and credentials. Hub consumes bounded
local summaries and/or notification-derived state and must not scrape private
provider databases or manufacture delivery/read semantics.

The preserved-data physical device intentionally kept user-installed Play state,
so clean-baseline AppStore claims are not made from that device. Appearance and
other polish issues remain open follow-ups and will be closed with Titan 2
SableOS install work where they are proven or superseded.

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

The deployment architecture is split:

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
| Pixel 7 / panther | REFERENCE_FROZEN | accepted R9 Hub V1 reference | maintenance/regression only |
| Titan 2 | PORTABILITY | N0 active | keyboard/display/camera/restore/radio research in parallel |
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

## Open issue policy

Open issues are intentionally kept open until Titan 2 SableOS install closure.
The open set covers active Titan qualification and keyboard-first work plus
physical/polish follow-ups that should be proven or superseded by the Titan 2
install path.

## Current execution order

```text
R9 Hub V1 Panther closure merged
  -> keyboard-first design V1 merged
  -> organization documentation reconciliation
  -> Titan 2 read-only evidence / restore / input / display / camera / radio
  -> Titan 2 first controlled Sable artifact + deployment qualification
  -> close remaining open issues where Titan 2 proves or supersedes them
  -> Titan 2 Elite independent sequence
  -> Q27 only after shipped hardware qualifies
  -> production signing / OTA release engineering later
```
