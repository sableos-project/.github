# Current cross-repository authority

Status: **current organization authority — 2026-10-02 ET / 2026-10-03 UTC**

Historical evidence is preserved. Current/normative documents must not conflict
with this authority map.

## Current authority chain

| Responsibility | Repository | Current ref / exact head |
| --- | --- | --- |
| private integration / current execution | `aimindseye/sableos` | `main` / `dea340c7f03f9b0c18e2b0b7b60eebf80b293d9c` |
| parallel Titan product-source candidate | `aimindseye/titan2-temp` | `main` / `dbbb96cc01a0e366ea56817b54028b5686cb4035` |
| public common platform/design | `sableos-project/platform_sable` | `main` / `80e3097bdef4cd2d5cbfd8ebde16390c29767918` |
| public Sable Start presentation/history | `sableos-project/packages_apps_SableStart` | `main` / `771892d726f1220e5c1f2ccb686134c9481c3d08` |
| public common product composition | `sableos-project/vendor_sable` | `main` / `82aee9331f2819457ae99f650e0f56f7b7a7d658` |
| public exact source/composition model | `sableos-project/platform_manifest` | `main` / `89191cd66b0b0b6d34927c258e33eb9bd56fb9d0` |
| public Titan 2 device boundary | `sableos-project/device_sable_titan2` | `main` / `e0f3153db7a248b9fe0ebadb3f7e7fa83939579e` |
| public build/deploy contracts | `sableos-project/build` | `main` / `e677f91f04aba648080c0e3028597ff74d52fcf1` |
| frozen Panther device reference | `sableos-project/device_sable_panther` | `main` / `1c69a82344aac26f3af5d4fbba17615716d17469` |
| Restless/Treble compatibility reference | `sableos-project/treble_restlessos` | `android-17.0` / `88007e8636dc567851b9fced539229ac15defbdb` |

## Current Titan 2 execution state

```text
TITAN2_ACTIVE_ENGINEERING_LANE=N1D_C3B
TITAN2_ANDROID_RELEASE=16
C3B_E1_SYSTEMIMAGE=PASS
C3B_E2_SOURCE_ADMISSION=PASS
C3B_E3_BUILD_SOURCE=caf98dde723d07a071d95aaa1ef27d578d3208d8
C3B_E3_STATUS=BUILD_RUNNING_NOT_YET_SEALED
C3B_RUNTIME_PATCH_ALLOWLIST_COUNT=0
PUBLIC_BUILD_IMAGE_AUTHORIZED=NO
PUBLIC_FLASH_AUTHORIZED=NO
PRODUCTION_SIGNING_AUTHORIZED=NO
```

The full RestlessOS runtime stack is not the Sable product/security baseline.

## Current product-source assignment

```text
P1=Sable Start keyboard-first handoff
P2=Sable Keyboard provisioning readiness
P3=SetupWizard2 keyboard/square-display integration preparation
P4=Weather city-management + keyboard-first closure
P5=Sable Reader v2 architecture accepted / P5A-P5F
```

P1-P4 proceed without per-stage ai-g732 waits but remain unmerged until batched
exact-head operator qualification. P5 architecture is accepted and may proceed as a separate sibling P5A-P5F implementation train.

## SableScreens / Reader v2 authority

```text
SABLESCREENS_REFERENCE_BASELINE=YES
SABLESCREENS_SCREEN_COUNT=23
SABLESCREENS_SHIPPING_RUNTIME=NO
P5_READER_V2_ARCHITECTURE=ACCEPTED
P5_TEXT_READER_BOUNDARY=SEPARATE
```

Common product architecture is published in `sableos-project/platform_sable/docs/SABLE_READER_V2_ARCHITECTURE.md` and `docs/SABLESCREENS_REFERENCE_BASELINE.md`.

## Current Sable Hub portability policy

Panther Hub V1 is the common-product reference for Connected Apps semantics.
Titan 2 / Titan 2 Elite may change presentation for keyboard-first interaction
but must not drop or replace those common semantics.

```text
SABLE_HUB_PANTHER_V1_SEMANTICS_REQUIRED=YES
SABLE_HUB_CONNECTED_APPS=REQUIRED
SABLE_HUB_GENERIC_NOTIFICATION_ADAPTER=REQUIRED
SABLE_HUB_REMOTEINPUT_REPLY=REQUIRED
SABLE_HUB_OPEN_APP_FALLBACK=REQUIRED
SABLE_HUB_PACKAGE_USER_POLICY=REQUIRED
SABLE_HUB_LOCAL_BOUNDED_HISTORY=REQUIRED
SABLE_HUB_DYNAMIC_PROVIDER_DISCOVERY=REQUIRED
SABLE_HUB_PROVIDER_SPECIFIC_ALLOWLIST=NO
SABLE_MESSAGES_AND_SABLE_HUB_ARE_SEPARATE_PRODUCT_SURFACES=YES
D4_MESSAGES_REPLACES_SABLE_HUB=NO
D4_MESSAGES_WEAKENS_CONNECTED_APPS=NO
```

WhatsApp, Signal, Telegram and LinkedIn are compatibility/evidence targets, not
a source-code allowlist. Sable Messages is a separate product surface and must
not replace, subsume, disable or weaken Sable Hub.

Public semantic authority belongs in
`sableos-project/platform_sable/docs/SABLE_HUB_PORTABILITY_CONTRACT.md`;
private integration owns source/gate enforcement and device runtime
qualification remains target-specific.

## Keyboard-first design closure authority

```text
DESIGN_KF_A_NOTIFICATION_ATTENTION_HUB=ACCEPTED
DESIGN_KF_B_SYSTEMUI_CONVERGENCE=ACCEPTED
DESIGN_KF_C_SABLE_TOOLS_CONSOLIDATION=ACCEPTED
DESIGN_KF_D_ALL_APPS_RESPONSIVE_POLISH=ACCEPTED
KEYBOARD_FIRST_V1_FOUNDATIONAL_DESIGN=COMPLETE
TITAN2_V1_PRODUCT_DESIGN=COMPLETE
```

Normative public contracts live under
`sableos-project/platform_sable/docs/design/` at
`80e3097bdef4cd2d5cbfd8ebde16390c29767918`.

Private future-assignment sequencing lives in
`aimindseye/sableos/docs/titan2/KEYBOARD_FIRST_DESIGN_IMPLEMENTATION_QUEUE.md`.

## Current HOME / IME policy

```text
SABLE_FIRST_PARTY_HOME_OWNER=Launcher3QuickStep
SABLESTART_ROLE=PRESENTATION_AND_STATE_SOURCE_HOSTED_IN_LAUNCHER3
STANDALONE_SABLELAUNCHER_RUNTIME=RETIRED
THIRD_PARTY_HOME_SELECTION_ALLOWED=YES
FORCE_SABLE_HOME_AFTER_USER_SELECTION=NO

SABLE_FIRST_PARTY_IME=SableKeyboard
SABLE_FACTORY_DEFAULT_IME=PENDING_CANONICAL_IR005
THIRD_PARTY_IME_SELECTION_ALLOWED=YES
FORCE_SABLE_IME_AFTER_USER_SELECTION=NO
```

## Synchronization rule

A cross-repository product/architecture decision is complete only when the
private integration authority and every affected public role repository agree.
Historical documents stay preserved and are classified as historical rather than
silently rewritten.

The current public roadmap is `docs/CURRENT_ROADMAP_AND_ASSIGNMENTS.md`.
