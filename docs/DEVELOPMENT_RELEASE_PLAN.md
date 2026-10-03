# SableOS development plan

Status: **current execution plan — 2026-10-02 ET / 2026-10-03 UTC**

Internal labels are engineering/qualification milestones, not semantic public
release versions.

## Completed foundations

- Panther R9 Hub V1 physical closure — frozen touch-first reference.
- Keyboard-first architecture V1.
- K1/K2 multi-device artifact/deployment foundation.
- Titan 2 C3A public-functional control.
- Titan 2 C3B E1 minimal Sable baseline systemimage.
- Titan 2 C3B E2 five-module source admission.

## Active phase — Titan 2 C3B E3

E3 is the first Sable-composed Titan 2 engineering systemimage.

```text
BUILD_SOURCE=caf98dde723d07a071d95aaa1ef27d578d3208d8
PRODUCT=sable_titan2
LUNCH=sable_titan2-bp4a-userdebug
STATUS=BUILD_RUNNING_NOT_YET_SEALED
RUNTIME_PATCH_ALLOWLIST_COUNT=0
```

The long build started only after cheap compatibility/product/module gates
passed. Device contact and flash remain unauthorized.

## Parallel product train

The developer continues in `aimindseye/titan2-temp`:

1. P1 — Launcher3-hosted Sable Start keyboard-first source/handoff.
2. P2 — Sable Keyboard first-boot/default-provisioning readiness.
3. P3 — GrapheneOS SetupWizard2 keyboard/square-display integration preparation.
4. P4 — Weather user-managed cities and keyboard-first closure.

P1-P4 proceed continuously. Each stage is frozen. After P4, all four exact heads
receive one batched ai-g732 qualification session before merge/admission.

P5 Sable Reader v2 architecture is accepted and proceeds as the separate P5A-P5F implementation train; Text Reader remains separate.

## Completed keyboard-first design closure

```text
DESIGN-KF-A  Notification Policy + Attention + Hub ownership        ACCEPTED
DESIGN-KF-B  SystemUI visual convergence                            ACCEPTED
DESIGN-KF-C  Sable Tools / Toolbox consolidation                    ACCEPTED
DESIGN-KF-D  All Apps privacy row + responsive app polish           ACCEPTED
```

No additional broad design phase is planned before implementation. These packages wait for the current P1-P4 and P5 developer trains to finish and qualify.

## Engineering sequence after E3

```text
E4  seal artifact + image membership + partition/restore/fastbootd readiness
E5  controlled first physical C3B boot
E6  untouched runtime baseline: radio/input/display/camera/setup/security
E7  smallest evidence-backed compatibility changes
E8A core OS usability closure
E8B daily-driver apps/privacy closure
E8C evidence-gated hardware enhancements
N1D Beta 1 integrated daily-driver candidate
Release engineering later
```

## Historical plan classification

N0/N1A/N1B/N1B2/N1B3/N1C documents remain historical evidence and may provide
requirements or deployment lessons. They are not current execution authority.

## Stop conditions

Do not:

- treat E3 build progress as E3 PASS;
- authorize device contact or flash from public documentation;
- import the Restless patch stack wholesale;
- fork common apps by device model;
- create a second HOME or SetupWizard runtime;
- force Sable HOME/IME after explicit user choice;
- claim production signing or OTA readiness.
