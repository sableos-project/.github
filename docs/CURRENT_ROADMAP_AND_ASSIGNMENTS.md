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
aimindseye/sableos main=107464223295b29e2f8a04738d804abe47249603
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

P5 architecture is accepted:

```text
Sable Reader v2
  books / ebooks / PDF-style reading
  comics / manga / webtoons
  audiobooks

Sable Text Reader
  remains a separate lightweight text/TTS/accessibility product
```

Comic direction is local-first and may use Mihon as product/UX inspiration without importing Mihon runtime or an executable extension ecosystem.

P5 sequence:

```text
P5A  baseline/privacy/dependency cleanup + common library model
P5B  keyboard-first EPUB/PDF UX
P5C  CBZ/image comic engine + manga/webtoon UX
P5D  audiobook engine + background MediaSession
P5E  unified backup/collections/progress + quality gates
P5F  exact-head ai-g732 qualification
```

The existing 23-screen SableScreens catalog is an explicit design/reference baseline. It does not replace canonical Launcher3, SystemUI, Keyguard, Settings, Telecom or SetupWizard2 runtime owners. Weather and the dedicated Reader v2 product surface are additive to that catalog.

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
