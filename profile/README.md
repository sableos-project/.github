# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible
mobile OS built around one common product core, narrow privilege boundaries and
separate touch-first / keyboard-first interaction profiles.

> Applications provide capabilities; Sable organizes people, attention and actions.

## Current state — 2026-10-02 ET / 2026-10-03 UTC

```text
Pixel 7 / Panther      R9 PHYSICAL ACCEPTANCE PASS / FROZEN TOUCH-FIRST REFERENCE
Titan 2                ACTIVE N1D/C3B RUNTIME RECOVERY; N1B CURRENT-FIRMWARE CONTROL NEXT
Titan 2 Elite          INDEPENDENT KEYBOARD-FIRST TARGET PENDING
Q27                    RESEARCH / FUTURE PRODUCT CANDIDATE
Local direct CI        ACTIVE
Production signing     DEFERRED
```

Current private integration authority:

```text
PRIVATE_MAIN=0b3f3439e4df34644131fa43bd6a9cf9cf7f8ba0
C3B_E3_BUILD_SOURCE=caf98dde723d07a071d95aaa1ef27d578d3208d8
C3B_E3_STATUS=SEALED_PASS
```

Panther remains the accepted touch-first reference:

```text
R9_PANTHER_ACCEPTED_SOURCE=edf62e5bb08372a1395841d6cc5d78d3148a7695
R9_PANTHER_TARGET_FILES_SHA256=a0b359613c4f30e9a834fba212e0b044a97d63ed0537c59471c31b99b627d285
R9_PANTHER_PHYSICAL_ACCEPTANCE=PASS_WITH_PRESERVED_PLAY_STATE
```

Titan 2 C3B E3 remains a sealed build artifact with system SHA-256
`998a8b99cd4d1a631006291a96c6f0160c5a4400c4bf6bba62c319c4ed3c82a5`.
The first private N1D physical attempt did not establish a stable boot and entered
a reboot loop. The current recovery plan does not treat the `COUNT=0`
compatibility decision as a proven root cause: the next physical control is the
exact sealed N1B system artifact on current V01.00.14. Only if that boots is a
minimal N1E build authorized; N1E retains the N1B product/lunch identity and adds
only the five C3B apps. If N1B fails, N1E stops and firmware/boot-chain/current-
state differences are investigated. Public repositories do not themselves
authorize device contact or flashing.

## Current roadmap

Parallel product work:

```text
P1  Sable Start keyboard-first handoff
P2  Sable Keyboard provisioning readiness
P3  SetupWizard2 keyboard/square-display integration preparation
P4  Weather city-management + keyboard-first closure
P5  Sable Reader v2 architecture ACCEPTED / P5A-P5F implementation
```

P1-P4 proceed without per-stage operator waits and are later batch-qualified on
ai-g732 before canonical admission. P5 is now authorized as a separate sibling implementation train; it is not stacked into P1-P4.

The existing `titan2-temp/apps/titan2/screens` 23-screen catalog is now an explicit design/behavior baseline, not a shipping runtime. P1-P5 and E8 consume its applicable focus/privacy/theme/profile semantics.

Keyboard-first/Titan 2 V1 foundational design is now closed:

```text
DESIGN_KF_A_NOTIFICATION_ATTENTION_HUB=ACCEPTED
DESIGN_KF_B_SYSTEMUI_CONVERGENCE=ACCEPTED
DESIGN_KF_C_SABLE_TOOLS_CONSOLIDATION=ACCEPTED
DESIGN_KF_D_ALL_APPS_RESPONSIVE_POLISH=ACCEPTED
KEYBOARD_FIRST_V1_FOUNDATIONAL_DESIGN=COMPLETE
TITAN2_V1_PRODUCT_DESIGN=COMPLETE
```

Implementation remains queued until the current P1-P4 and P5 qualification trains finish.

Engineering path:

```text
N1D sealed build evidence
 -> N1D physical reboot-loop evidence
 -> exact sealed N1B control on current V01.00.14
 -> if N1B PASS: minimal N1E = N1B product + five C3B apps
 -> N1E G0/G1/G2 pre-flash qualification
 -> if N1E boots: resume controlled runtime/product deltas
 -> E6 runtime baseline
 -> E7 evidence-driven compatibility
 -> E8 product closure
 -> N1D Beta 1
 -> production release engineering later

if N1B FAIL:
 -> stop N1E and investigate firmware/boot-chain/current-device-state delta
```

See [Current roadmap and assignments](../docs/CURRENT_ROADMAP_AND_ASSIGNMENTS.md).

## Current developer-delivery checkpoint

`aimindseye/titan2-temp` P5E feature baseline is `4089c1274a9e4a402a2c617cc00a7c166112f195`; later docs-only commits do not change that qualified feature baseline. Two follow-up PRs are intentionally draft:

```text
PR_28=P5F Reader runtime-readiness
PR_28_OPERATOR_ANDROID_QUALIFICATION=PENDING
PR_28_DEVICE_RUNTIME=NOT_RUN

PR_29=non-Reader C3B runtime-readiness/handoff tooling
PR_29_STATIC_MUTATION=PASS_DEVELOPER
PR_29_OPERATOR_ANDROID_OFFLINE=PENDING
PR_29_DEVICE_RUNTIME=NOT_RUN
```

Neither draft is canonical runtime admission and neither changes the N1B -> N1E
recovery sequence.

## Product architecture

```text
common Sable applications + semantic contracts
        |
        +-- touch-first profile      -> Panther reference
        |
        +-- keyboard-first profile   -> Titan 2 / Titan 2 Elite / future Q27
        |
        v
bounded device adapters
        |
        v
Android framework + vendor HAL/BSP + hardware
```

Current first-party HOME is Launcher3/Launcher3QuickStep hosting Sable Start
presentation/state. The standalone SableLauncher runtime is retired. Android user
choice of another launcher or IME remains supported.

## Repositories

- **platform_manifest** — exact source/composition model.
- **platform_sable** — common semantic/design/portability contracts.
- **vendor_sable** — common product composition.
- **device_sable_panther** — frozen Pixel 7 reference adapter/evidence.
- **device_sable_titan2** — public Titan 2 device boundary/evidence.
- **build** — public build/evidence/deployment contracts.
- **packages_apps_SableStart** — Sable Start presentation/history reference.
- **treble_restlessos** — compatibility/reference fork only; not Sable runtime/security authority.
- **.github** — organization status, roadmap and documentation authority.

## Public release boundary

```text
PUBLIC_TITAN_BUILD_IMAGE=NO
PUBLIC_TITAN_FLASH=NO
PRODUCTION_SIGNING_AUTHORIZED=NO
PUBLIC_RELEASE_AUTHORIZED=NO
```

Historical N0/N1B/N1B2/N1B3/N1C documents remain evidence, not current
execution authority.
