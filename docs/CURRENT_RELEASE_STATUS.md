# SableOS current development release status

Status date: **2026-09-20**

This is the organization-level current-state snapshot. Historical R5–R8 requirement and evidence documents remain valid records, but this document defines the current execution state when older milestone wording conflicts with it.

![Local CI](https://img.shields.io/badge/CI-local%20direct-active-2ea44f)
![R9 Launcher](https://img.shields.io/badge/R9%20launcher%20visual-PASS-2ea44f)
![Product composition](https://img.shields.io/badge/product%20composition-PASS-2ea44f)
![Fresh Panther](https://img.shields.io/badge/fresh%20Panther%20build-IN%20PROGRESS-f0ad4e)
![Pixel 7](https://img.shields.io/badge/Pixel%207%20physical-PENDING-lightgrey)
![Titan 2](https://img.shields.io/badge/Titan%202-keyboard--first%20QUEUED-6f42c1)

## Current release train

The current development release is **R9**.

R8 established the shared design/application/product-integration foundation. R9 is closing the launcher/home architecture and then proving the resulting full Panther product from a fresh source-bound Android output before physical-device acceptance.

```text
R8
  shared design + first-party app suite + product composition foundation
  Calculator / Sudoku / Minesweeper / 2048 / Media / Reader / Hub / Mail
        |
        v
R9-L
  Launcher3/Quickstep foundation
  + Sable Start production presentation
  + visual review
        |
        v
R9-P
  fresh source-bound Panther full-product build
        |
        v
R9-D
  physical Pixel 7 HOME / Overview / Recents / runtime acceptance
        |
        v
Titan 2
  keyboard-first N0 portability/GSI lab
        |
        v
later production signing / OTA release engineering
```

The internal R* names are development/validation milestones, not semantic public product versions.

## Current gate state

| Gate | State | Current claim |
| --- | --- | --- |
| Local direct CI | **ACTIVE** | canonical source/static/app qualification runs on the controlled build machine |
| R9 Launcher3 foundation | **PASS** | Launcher3/Quickstep owns HOME/Overview/Recents; Sable provides the NORMAL-state presentation |
| R9 Sable Start visual set | **PASS** | Start, All Apps, rail active state, Search, Peek, Local Context, Appearance, light/dark, alternate accent reviewed |
| Standalone SableStart HOME APK | **RETIRED** | product target is Launcher3QuickStep; standalone SableStart product package is absent |
| R8 first-party app composition | **PASS** | target-files composition has proven the eight-app set and package identities |
| Incremental Panther product build | **PASS** | warmed-output product/target-files composition is healthy |
| Fresh Panther full-build causality | **IN PROGRESS** | must use an initially absent source-bound OUT and produce fresh target-files |
| Pixel 7 R9 physical acceptance | **PENDING** | flashing/device mutation waits for fresh-build proof |
| Titan 2 stock read-only inventory | **ALLOWED WHEN HARDWARE AVAILABLE** | observational T0 work may run without mutation |
| Titan 2 GSI/flash | **QUEUED** | blocked until Panther R9 physical acceptance |
| Production signing / OTA | **DEFERRED** | not on the current development critical path |

The earlier 53-second Panther target request is retained as useful **incremental product-composition evidence**, not as proof of a fresh full rebuild.

## Launcher architecture

R9 deliberately moved away from a standalone custom HOME implementation.

```text
Launcher3 AllAppsStore
    product inventory/update authority

Launcher3/Quickstep
    HOME role
    Overview/Recents
    task/gesture integration
    lifecycle/state authority

SableStartScreen
    Start
    All Apps + Sable Rail
    Search / Command
    Sable Peek
    Local Context
    Appearance
```

Product HOME identity:

```text
package:  com.android.launcher3
activity: com.android.launcher3.sable.SableQuickstepLauncher
module:   Launcher3QuickStep
```

The old standalone SableStart HOME APK is not part of the product graph.

## CI and build execution model

SableOS no longer uses GitHub-hosted Actions as the authoritative build/qualification path.

```text
GitHub
  source hosting
  code review
  issues / planning
  documentation

controlled local build machine
  canonical host toolchain
  build/local-ci/run.sh
  source-bound evidence
  trusted application builds
  Soong/product integration
  Panther full product builds
```

A GitHub self-hosted runner is also **not active**. The earlier trusted-runner plan remains deferred until storage encryption/isolation and runner trust requirements are explicitly satisfied.

The operator-facing release build interface is release-neutral:

```bash
bash build/panther/run-release.sh R9
```

The release identifier is the release-specific input. Do not create a new build script or a new /tmp driver command sequence for every milestone.

## Fresh-build rule

A successful Ninja/Soong target request against an existing output directory proves target satisfiability, not fresh-build causality.

For R9 release qualification:

```text
out_sable_r9_full_<source-sha12>
```

must be absent before the build starts. The release runner and inner build both fail closed if it already exists. Target-files freshness must be newer than the recorded build start.

Required closing evidence:

```text
R9_RELEASE_CANDIDATE_FRESH_OUT=PASS_ABSENT
R9_FRESH_FULL_BUILD_OUT_PRECONDITION=PASS_ABSENT
R9_TARGET_FILES_FRESHNESS=PASS
R9_FRESH_FULL_BUILD=PASS
R9_FULL_BUILD_CAUSALITY=PASS_ABSENT_OUT_TO_FRESH_TARGET_FILES
```

## Panther and Titan 2

**Pixel 7 / panther** remains the authoritative full-stack Android 17 development and security target.

**Titan 2** is the next portability target, classified initially as a keyboard-first N0 GSI/userspace lab:

```text
navigation.primary=keyboard
navigation.secondary=touch
```

Titan acceptance must explicitly cover physical-keyboard focus, printable-key type-to-search, Enter/Back behavior, HOME navigation, pointer/touch coexistence and square/compact layout behavior. Panther touch-first success does not imply Titan 2 success.

Titan 2 mutation/flash remains blocked until the R9 Panther fresh build and physical runtime gates close.

## Release plan after Panther R9

After the Panther R9 launcher/runtime closure:

1. run Titan 2 T0 read-only stock/device inventory when hardware is available;
2. qualify the keyboard-first interaction profile and a bounded N0 GSI path;
3. carry forward the same common Sable application/product contracts without device-name forks;
4. continue broader Sable Flow/Hub/productivity work on the common core;
5. return production signing/OTA/key-custody engineering to the critical path only when development qualification is mature enough to justify a release candidate.

## Documentation authority

Use this document together with:

- `DEVELOPMENT_RELEASE_PLAN.md` — current milestone sequence and ownership;
- `CI_TRUST_ARCHITECTURE.md` — local CI / trusted build / device / signing boundaries;
- `DOCUMENTATION_STATUS.md` — current-vs-historical document map;
- `SECURITY_QUALITY_ENGINEERING.md` — engineering-assurance requirements.

Historical evidence is preserved. It is not rewritten to make old milestones look current.
