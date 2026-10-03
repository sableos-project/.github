# SableOS current roadmap and assignments

Status: **current organization roadmap**
Updated: **2026-10-02 ET / 2026-10-03 UTC**

This is the public organization mirror of the current private execution plan.
Exact build/runtime authority remains in `aimindseye/sableos`.

## Current state

```text
Panther           R9 frozen touch-first reference
Titan 2           active N1D/C3B E3 engineering target
Titan 2 Elite     independent portability target pending
Q27               research / future candidate
Production signing deferred
```

Current private integration authority:

```text
aimindseye/sableos main=26d11bed93ef4eab924fc63100113bd94e8ae88b
C3B_E3_BUILD_SOURCE=caf98dde723d07a071d95aaa1ef27d578d3208d8
C3B_E3_STATUS=BUILD_RUNNING_NOT_YET_SEALED
```

C3B E1 systemimage qualification and E2 source admission are complete. E3 is the
first Sable-composed Titan 2 systemimage build. It entered the long build only
after compatibility, product, parse and five-module qualification passed.

Public Titan build/flash/signing/release remain closed.

## Parallel assignments

Developer source work remains in `aimindseye/titan2-temp`.

```text
P1  Launcher3-hosted Sable Start keyboard-first source/handoff
P2  Sable Keyboard first-boot/default-provisioning readiness
P3  GrapheneOS SetupWizard2 keyboard/square-display integration preparation
P4  Sable Weather city-management + keyboard-first closure
```

P1-P4 proceed continuously without waiting for ai-g732 between stages. Each
stage is frozen separately; after P4 the operator runs one batched exact-head
ai-g732 qualification before canonical admission.

P5 is currently design/scope work only:

```text
Sable Reader v2
  books / ebooks / PDF-style reading
  comics / manga / webtoons
  audiobooks

Sable Text Reader
  remains a separate lightweight text/TTS/accessibility product
```

Comic direction is local-first and may use Mihon as product/UX inspiration
without importing an unreviewed executable extension ecosystem.

## Engineering roadmap

| Phase | Purpose | State |
| --- | --- | --- |
| E3 | first Sable-composed Titan 2 systemimage | RUNNING / not sealed |
| E4 | artifact seal + deployment readiness | NEXT |
| E5 | first controlled physical C3B boot | NEXT |
| E6 | runtime baseline: radio/input/display/camera/setup/security | after E5 |
| E7 | smallest evidence-backed compatibility changes | after E6 |
| E8 | product-closure waves | parallel source work / later integration |
| N1D Beta 1 | integrated daily-driver candidate | later |
| Release | production signing / OTA / public release | deferred |

## Safety boundaries

```text
PUBLIC_TITAN_BUILD_IMAGE=NO
PUBLIC_TITAN_FLASH=NO
DEVICE_CONTACT_AUTHORIZED_BY_PUBLIC_REPOS=NO
PRODUCTION_SIGNING_AUTHORIZED=NO
PUBLIC_RELEASE_AUTHORIZED=NO
```

## Historical plan rule

N0, N1A, N1B, N1B2, N1B3 and N1C documents are historical evidence and
requirements/deployment history. They are not the current execution roadmap.
