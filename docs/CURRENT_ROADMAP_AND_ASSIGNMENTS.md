# SableOS current roadmap and assignments

Status: **current organization roadmap**
Updated: **2026-10-03 ET**

This is the public organization mirror of the current private execution plan.
Exact build/runtime authority remains in `aimindseye/sableos`.

## Current state

```text
Panther           R9 frozen touch-first reference
Titan 2           active N1D/C3B runtime recovery; N1B current-firmware control next
Titan 2 Elite     independent portability target pending
Q27               research / future candidate
Production signing deferred
```

Current private integration authority:

```text
aimindseye/sableos main=f08aac781e58b549e63d7104d495993917d43353
C3B_E3_BUILD_SOURCE=caf98dde723d07a071d95aaa1ef27d578d3208d8
C3B_E3_STATUS=SEALED_PASS
```

C3B E1 systemimage qualification and E2 source admission are complete. E3
remains a sealed build artifact, and E4/E5A remain valid deployment/read-only
evidence. The first private N1D physical attempt did not establish a stable boot
and entered a reboot loop, so E6 is frozen. The next physical control is the
exact sealed N1B system artifact on current V01.00.14. A minimal N1E build is
authorized only if that control boots.

Public Titan build/flash/signing/release remain closed.

## Runtime-recovery control

```text
N1D_PHYSICAL_RESULT=FAIL_REBOOT_LOOP
N1D_RUNTIME_PATCH_ALLOWLIST_COUNT=0
COUNT_ZERO_STATUS=PRIMARY_SUSPECT_NOT_PROVEN_ROOT_CAUSE
N1B_V010014_CONTROL=PENDING
N1E_BUILD_AUTHORIZED=NO_PENDING_N1B_CONTROL
N1E_FLASH_AUTHORIZED=NO
E6_RUNTIME_BASELINE=HOLD
```

The control is intentionally narrow. If N1B boots on current firmware, N1E keeps
the exact N1B generated `treble_arm64_bvN` product/lunch identity and adds only
the five C3B apps. It does not use the `sable_titan2` wrapper identity. N1E
must pass source/config/artifact qualification before any flash. If N1B fails,
N1E stops and the firmware/boot-chain/current-device-state delta is investigated.

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

P5 v2.0 is local-only:

```text
P5_V2_INTERNET_PERMISSION=ABSENT
P5_OPDS=P5_1_DEFERRED
```

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

## Current developer-delivery status

The current product-source main is
`aimindseye/titan2-temp@4089c1274a9e4a402a2c617cc00a7c166112f195`
after the accepted P5E merge.

```text
PR_28=P5F_READER_RUNTIME_READINESS_DRAFT
PR_28_HEAD=e8a5ddd374a0d78ca175b2137cf73707150ca28a
PR_28_OPERATOR_ANDROID_COMPILE_LINT_TEST_MANIFEST=PENDING
PR_28_DEVICE_RUNTIME=NOT_RUN

PR_29=C3B_NON_READER_RUNTIME_READINESS_DRAFT
PR_29_HEAD=5468758dc42c372854fc6936e3800afc1ee33def
PR_29_STATIC_MUTATION=PASS_DEVELOPER
PR_29_OPERATOR_ANDROID_OFFLINE=PENDING
PR_29_DEVICE_RUNTIME=NOT_RUN
```

Both remain draft until exact-head operator qualification is attached. Neither
PR is canonical runtime admission, and neither changes the N1B -> N1E recovery
order.

## Keyboard-first design closure

The following contracts are accepted and implementation-ready:

```text
DESIGN-KF-A  Notification Policy + Attention + Hub ownership
DESIGN-KF-B  SystemUI visual convergence
DESIGN-KF-C  Sable Tools / Toolbox consolidation
DESIGN-KF-D  All Apps privacy row + responsive app polish
```

Authority: `sableos-project/platform_sable@80e3097bdef4cd2d5cbfd8ebde16390c29767918`.

They remain intentionally unassigned until:
- Developer A completes and operator-qualifies P1-P4;
- Developer B completes P5A-P5E and operator-qualifies P5F.

Recommended later handoff:
`Developer A -> KF-D-I -> KF-A-I -> KF-B-I`;
`Developer B -> KF-C-I1..I5`.

## P1-P4 prequalification status

Developer A completed the stacked P1-P4 source train, but operator review found one normalized-key correction required before ai-g732 qualification.

```text
P1_P4_PREQUAL_RESTACK_REQUIRED=YES
P1_MOVE_HOME_SYSTEM_HOME_CONFLATION=FIX_REQUIRED
NORMALIZED_KEY_CONTRACT=80e3097bdef4cd2d5cbfd8ebde16390c29767918
AI_G732_P1_P4_BATCH_QUALIFICATION=HOLD_UNTIL_RESTACK
```

No device/runtime/release claim is affected.

## E3 sealed artifact

```text
N1D_C3B_E3_SYSTEMIMAGE=PASS
SYSTEM_SHA256=998a8b99cd4d1a631006291a96c6f0160c5a4400c4bf6bba62c319c4ed3c82a5
SYSTEM_BYTES=2994405376
MODULES_IN_SYSTEM_TREE=PASS count=5
DEVICE_CONTACT_AUTHORIZED=NO
RELEASE_ELIGIBLE=NO
```

E4 deployment readiness is sealed PASS. E5A read-only device-state revalidation is sealed PASS; E5B flash/mutation remains unauthorized.

## E4 sealed / E5 authorization boundary

```text
N1D_C3B_E4_DEPLOYMENT_READINESS=PASS_REVIEW_READY
N1D_C3B_E4_DIRECT_FIT=NO
N1D_C3B_E4_PLANNED_SYSTEM_TARGET_BYTES=3263168512
N1D_C3B_E4_ESTIMATED_REMAINING_SUPER_HEADROOM_BYTES=3003637760
N1D_C3B_E4_STOCK_RESTORE_PROOF=PASS
E5A_RESULT=PASS_REVIEW_READY_SEALED
E5A_FINAL_DEVICE_MODE=FASTBOOTD
E5A_ALLOCATION_COMPLETE=YES
E5A_CAPACITY_SAFE=YES
E5A_REMAINING_HEADROOM_AFTER_PLANNED_GROWTH_BYTES=2954141696
E5B_FLASH_AUTHORIZED=NO
E5B_LP_MUTATION_AUTHORIZED=NO
E5B_AVB_MUTATION_AUTHORIZED=NO
E5B_SLOT_MUTATION_AUTHORIZED=NO
```

Private E5A execution completed cleanly. Historical Titan 2 evidence confirms the dynamic userspace is Virtual A/B with active logical materialization: a zero-sized/non-openable `system_b` while slot A is active is not an inactive deployment target. The corrected E5B package is bound to the proven N1B current-slot `system_a` path with COW/group-capacity review; N1C whole-`super` writing remains negative evidence.

```text
E5A_RESULT=PASS_REVIEW_READY_SEALED
E5A_FINAL_DEVICE_MODE=FASTBOOTD
E5B_AUTHORIZATION_PACKAGE=OFFLINE_PREPARATION_READY
E5A_REBOOT_BACK_TO_ANDROID=NO_NOT_AUTHORIZED
E5B_DEPLOYMENT_MODEL=ACTIVE_CURRENT_SYSTEM_VIRTUAL_AB
E5B_TARGET_LOGICAL_PARTITION=system_a
E5B_SLOT_POLICY=KEEP_CURRENT_SLOT_NO_SLOT_SWITCH
E5B_MUTATION_AUTHORIZED=NO
```

E5B mutation/first boot still requires another explicit authorization after
E5A evidence review.

## Engineering roadmap

| Phase | Purpose | State |
| --- | --- | --- |
| N1D evidence | sealed build + failed physical boot evidence | **RECORDED / N1D REFLASH HOLD** |
| N1B control | exact sealed N1B on current V01.00.14 | **NEXT PHYSICAL CONTROL** |
| N1E | exact N1B product + five C3B apps only | **BLOCKED UNTIL N1B PASS** |
| N1E qualification | G0/G1/G2 source/config/artifact isolation | **DESIGN REVIEWED; REAL HOST RUN PENDING** |
| E6 | runtime baseline: radio/input/display/camera/setup/security | **HOLD UNTIL BOOT-QUALIFIED BASELINE** |
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
