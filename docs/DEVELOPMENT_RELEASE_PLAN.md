# SableOS development plan

Status: **current execution plan — 2026-09-24**

Internal R-labels are development/qualification milestones, not semantic public
release versions. Exact engineering claims remain bound to source, artifact and
physical evidence.

## Completed foundation

```text
R8
  common design/application/product foundation

R9 Panther
  standalone SableLauncher HOME
  Quickstep Recents-only
  source-bound full CI
  fresh full image
  controlled preserved-data flash
  physical runtime/branding/appearance acceptance
  REFERENCE_FROZEN

K1
  multi-device artifact registry v2
  legacy Panther registry compatibility
  generic artifact kinds + manifest verification

K2
  common deployment safety/evidence orchestration
  adapter-owned transport/partition semantics
  Panther A/B fastboot behavior moved behind panther adapter
  Titan/Elite/Q27 remain fail-closed
```

Historical R8 build-plan/review files are retained for provenance. They no longer
describe the active execution order.

## Active phase — keyboard-first common product

Active product design now targets keyboard-first devices while preserving one
common Sable application/semantic core.

```text
common Sable semantics
    |
    +-- touch-first profile      -> frozen Panther reference
    |
    +-- keyboard-first profile   -> Titan 2 / Titan 2 Elite / future Q27
    |
    v
bounded device adapters
```

The first keyboard-first design tranche covers:

1. SableLauncher Start/Home/All Apps/Search/Peek geometry;
2. deterministic focus and focus restoration;
3. type-to-search and command/shortcut model;
4. keyboard-only navigation and accessibility;
5. square/near-square responsive layouts;
6. SystemUI/keyguard/notification interaction implications;
7. Sable Keyboard common IME boundary;
8. Sable Camera keyboard-first/system-image boundary;
9. application-wide keyboard interaction contract.

## Titan 2

Titan 2 is the active PORTABILITY/N0 research target.

Parallel research should now favor evidence that fills future adapter fields:
physical input pipeline, keyboard touch/mouse mode, display/input topology,
rear SubScreen ownership, stock keyboard/vendor-service ownership, restore/
fastbootd/super/AVB strategy and stock runtime parity baselines.

The first SableOS N0 deployment must preserve stock kernel/vendor/ODM/firmware
unless evidence requires otherwise.

No Titan mutation is enabled by K1/K2.

## Titan 2 Elite

Titan 2 Elite is an independent target, not a Titan 2 variant assumed equivalent
by name. When hardware is available, create a separate stock/boot/AVB/partition/
input/display/camera/telephony baseline before enabling any adapter capability.

A Titan 2 PASS never implies Elite PASS.

## Q27

Q27 remains RESEARCH/future PRODUCT_CANDIDATE. Do not activate build/flash
support from prototype/community evidence alone.

## Multi-device build/deployment contract

Canonical operator interface:

```text
build/sable.sh <device> <release> <function> [options]
```

Build/artifact identity never contains a physical serial.

Serial is required only for device-contact operations and must be explicitly
selected. Multiple attached devices are permitted; tooling must never fall back
to the first attached device.

## Non-Pixel assurance

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

A successful GSI boot is not N1 or N2.

## Production release work

Production application signing, AVB key hierarchy, OTA signing/update service,
rollback policy, signing-host custody and public release support remain a later
program. They do not block keyboard-first N0 engineering.

## Stop conditions

Do not:
- reopen Panther feature development by default;
- enable Titan/Elite/Q27 mutation before adapter/restore evidence exists;
- fork common applications by device model;
- treat a historical warmed OUT as fresh-build evidence;
- treat generic artifact-schema support as device release qualification;
- publish private firmware/evidence/serials through public repositories.
