# SableOS current development status

Status date: **2026-10-02 ET / 2026-10-03 UTC**

This file is the organization-level current-status authority. Historical
milestone documents remain evidence but do not override this state.

## Executive state

```text
Panther                   R9 REFERENCE_FROZEN / PHYSICAL PASS
Titan 2                   ACTIVE N1D/C3B E3 ENGINEERING
Titan 2 Elite             INDEPENDENT TARGET PENDING
Q27                       RESEARCH
Production signing        DEFERRED
```

## Exact current identities

```text
PRIVATE_INTEGRATION_MAIN=62fdcbb5f10a87eed37b66a32f6b6b687671e147
PARALLEL_PRODUCT_MAIN=dbbb96cc01a0e366ea56817b54028b5686cb4035

R9_PANTHER_IMAGE_SOURCE=edf62e5bb08372a1395841d6cc5d78d3148a7695
R9_PANTHER_TARGET_FILES_SHA256=a0b359613c4f30e9a834fba212e0b044a97d63ed0537c59471c31b99b627d285
R9_PANTHER_PHYSICAL_ACCEPTANCE=PASS_WITH_PRESERVED_PLAY_STATE

C3B_E1_SYSTEMIMAGE=PASS
C3B_E1_SYSTEM_SHA256=9ce0144220b3a5bf531a1b0da7ec97543a89c9181ab9d69b6390ea5a86ad45dd
C3B_E2_SOURCE_ADMISSION=PASS
C3B_E3_BUILD_SOURCE=caf98dde723d07a071d95aaa1ef27d578d3208d8
C3B_E3_STATUS=BUILD_RUNNING_NOT_YET_SEALED
C3B_RUNTIME_PATCH_ALLOWLIST_COUNT=0
```

E3 passed compatibility/product/module prerequisites before entering the long
fresh build. It is not yet a sealed artifact and has not been run on a device.

## Current assignments

```text
P1  Sable Start keyboard-first handoff
P2  Sable Keyboard provisioning readiness
P3  SetupWizard2 keyboard/square-display integration preparation
P4  Weather city-management + keyboard-first closure
P5  Reader v2 architecture ACCEPTED / P5A-P5F implementation
```

P1-P4 proceed as a frozen stacked developer train and receive batched exact-head
ai-g732 qualification after P4.

Reader v2 is one local-first keyboard-first library for EPUB/PDF, CBZ comics/manga/webtoons and audiobooks; Text Reader stays separate. Remote AI/account sync is excluded from P5.

## Next engineering phases

```text
E3 -> E4 artifact/deployment readiness
   -> E5 first physical C3B boot
   -> E6 runtime baseline
   -> E7 evidence-backed compatibility
   -> E8 product closure
   -> N1D Beta 1
```

## Safety

```text
PUBLIC_BUILD_IMAGE_AUTHORIZED=NO
PUBLIC_FLASH_AUTHORIZED=NO
DEVICE_CONTACT_AUTHORIZED_BY_PUBLIC_REPOS=NO
PRODUCTION_SIGNING_AUTHORIZED=NO
PUBLIC_RELEASE_AUTHORIZED=NO
```

N0/N1B/N1B2/N1B3/N1C are historical execution families, not current authority.
