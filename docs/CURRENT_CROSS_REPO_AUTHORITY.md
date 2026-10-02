# Current cross-repository authority

Status: **current organization authority — 2026-10-02**

## Purpose

This file exists to prevent a current implementation from being driven by a
historical README, old milestone plan or superseded repository-local architecture
note.

Historical evidence is preserved. Current/normative documents must not conflict
with this authority map.

## Current authority chain

Use these repositories for their stated responsibilities:

| Responsibility | Repository | Current ref / exact head |
| --- | --- | --- |
| private integration / current execution architecture | `aimindseye/sableos` | `main` / `648aec147765361676b51fb428b7ac177883f6f2` |
| parallel Titan product-source candidate | `aimindseye/titan2-temp` | `main` / `09617ecd2d1e4774fb8a8cd9e92ddae003abccde` |
| public common platform/design contracts | `sableos-project/platform_sable` | `main` / `5d0ca771f7d1c31975931dc4440d5997911d1432` |
| public Sable Start presentation/history source | `sableos-project/packages_apps_SableStart` | `main` / `1966857d2cfeb7dfcaa1821af61c0aa49db50869` |
| public common product composition | `sableos-project/vendor_sable` | `main` / `b9a4b5b0417e6c2e0d32822710424ad03fe50fe8` |
| public exact source/composition model | `sableos-project/platform_manifest` | `main` / `81b43ff7c8fbd68b798496e84e32d5b8ad0cc734` |
| public Titan 2 device boundary/evidence | `sableos-project/device_sable_titan2` | `main` / `e9bf74b21d238ebbdfdb44ac1c5e438dedc8ff66` |
| public build/deploy contracts | `sableos-project/build` | `main` / `0b66000f129a2de5821685ae175113e756914c70` |
| frozen Panther device reference | `sableos-project/device_sable_panther` | `main` / `8cd9be2b9520015958da810051753f8c4f14eaf7` |
| Restless/Treble compatibility reference | `sableos-project/treble_restlessos` | `android-17.0` / `d2fde7acd77029fabb951d181da6417800c4193c` |

The exact heads above identify the synchronized 2026-10-02 documentation state.
Normal development may advance `main`; reproduction must pin the exact revision
used by the build/evidence being reproduced.

## Current Titan 2 execution state

~~~text
TITAN2_ACTIVE_ENGINEERING_LANE=N1D_C3B
TITAN2_ANDROID_RELEASE=16
C3B_ENGINEERING_SYSTEMIMAGE=IN_PROGRESS_NOT_ACCEPTED
C3B_RELEASE_ELIGIBLE=NO
PUBLIC_BUILD_IMAGE_AUTHORIZED=NO
PUBLIC_FLASH_AUTHORIZED=NO
PRODUCTION_SIGNING_AUTHORIZED=NO
~~~

The earlier Titan N0/AOSP-first documents remain historical precursor/evidence
records. They are not current execution authority.

The active C3B architecture uses a Graphene/AOSP-derived Sable base with a
minimal Treble scaffold and a fail-closed compatibility-peel process. The full
RestlessOS runtime stack is not the Sable product/security baseline.

## Current product-source state

D1b product-source qualification is complete:

~~~text
D1B_ACCEPTED_HEAD=8e31cb253b9a6e720fed9a1be23fd6967c621317
D1B_MERGE_COMMIT=acec8e1c62b2aa5e2cf478df880149cd9f7274fc
D1B_ANDROID_COMPILE=PASS
D1B_ANDROID_TESTS=PASS
D1B_ANDROID_LINT=PASS
D1B_FULL_OFFLINE_QUALIFICATION=PASS
~~~

The later docs synchronization in `titan2-temp` is
`09617ecd2d1e4774fb8a8cd9e92ddae003abccde`.

Current next product work is D2 quality/dependency verification, D3 critical
text-entry hardening, D5 first-party icons, plus the non-blocking D6 keyboard
and Camera Control Deck roadmap.

## Current HOME policy

~~~text
SABLE_FIRST_PARTY_HOME_OWNER=Launcher3QuickStep
SABLESTART_ROLE=PRESENTATION_AND_STATE_SOURCE_HOSTED_IN_LAUNCHER3
STANDALONE_SABLELAUNCHER_RUNTIME=RETIRED
THIRD_PARTY_HOME_INSTALL_ALLOWED=YES
THIRD_PARTY_HOME_SELECTION_ALLOWED=YES
FORCE_SABLE_HOME_AFTER_USER_SELECTION=NO
~~~

"HOME owner" means Sable's canonical first-party implementation. It does not
mean SableOS blocks another launcher selected through Android's supported
default-HOME mechanism.

Third-party launchers do not automatically receive privileged
Quickstep/SystemUI/Private-Space integration.

## Current IME policy

~~~text
SABLE_FIRST_PARTY_IME=SableKeyboard
SABLE_FACTORY_DEFAULT_IME=SableKeyboard_PENDING_CANONICAL_INTEGRATION
THIRD_PARTY_IME_INSTALL_ALLOWED=YES
THIRD_PARTY_IME_ENABLE_ALLOWED=YES
THIRD_PARTY_IME_SELECTION_ALLOWED=YES
FORCE_SABLE_IME_AFTER_USER_SELECTION=NO
~~~

Sable Keyboard is the first-party critical-entry/direct-boot qualification path.
A third-party IME may be selected by the user but does not automatically inherit
Sable's pre-unlock, pairing, lockscreen or physical-keyboard acceptance claims.

## Current Sable Camera policy

Keyboard-first Sable Camera uses the **Camera Control Deck** product direction:

> **The screen is the viewfinder. The physical keyboard is the camera control
> surface.**

~~~text
SABLE_CAMERA_SOURCE_DIRECTION=SABLE_OWNED_CAMERA2
PORTRAIT_SUPPORTED=YES
LANDSCAPE_CONTROL_DECK=YES
GLOBAL_LANDSCAPE_LOCK=NO
TOUCH_FALLBACK=REQUIRED
KEYBOARD_FOCUS_POINT_MOVE=REQUIRED
AF_LOCK=ROADMAP_DEVICE_EVIDENCE_REQUIRED
AE_LOCK=ROADMAP_DEVICE_EVIDENCE_REQUIRED
~~~

This is an interaction model, not a new camera HAL or capture mode.

GrapheneOS Camera/Open Camera/community camera work remain references where
documented; proprietary/modded GCam code is not a product dependency.

## Pastiera / Plektra intake

Pinned reviewed checkpoint:

~~~text
repository=https://github.com/palsoftware/pastiera
release=v0.86
commit=e7d8f27ecc7253e61690b5d34f110b25dc68bb16
release_date=2026-09-26
license=GPL-3.0
successor=https://github.com/pkb-rocks/plektra
~~~

Pastiera/Plektra is behavior/product reference input only.

~~~text
PASTIERA_SOURCE_IMPORTED=NO
PLEKTRA_SOURCE_IMPORTED=NO
GPL_SOURCE_COPIED_INTO_SABLE_KEYBOARD=NO
~~~

Accepted non-blocking D6 concepts include compact input state/candidate strips,
configurable SYM/emoji/snippet surfaces, local dictionaries/prediction after
privacy review, searchable Settings/deep links and Titan 2 Elite inset testing.

Sable does not adopt Shizuku as an IME dependency, persistent keyboard clipboard
history enabled by default or automatic unverified remote layout/dictionary
channels.

## Historical-document rule

A file may remain unchanged when it is explicitly historical evidence.

A file marked `CURRENT_*`, `current normative`, `current authority`,
`current status` or equivalent must be reconciled when architecture changes.

When a historical document conflicts with the current authority chain:

1. preserve it as evidence;
2. add a superseded/historical marker if ambiguity exists;
3. do not rewrite old evidence to pretend the later architecture existed;
4. use this file plus the owning repository's current/normative documents for
   new work.

## Synchronization rule

A cross-repository product/architecture decision is not complete until:

~~~text
private integration authority updated
parallel product source/roadmap updated where applicable
platform_sable current contract updated
vendor_sable composition updated when defaults/composition change
platform_manifest updated when source/composition authority changes
device repo updated when target interpretation changes
.github current authority/status updated
historical evidence left intact or explicitly marked superseded
~~~

Build/device code is not modified merely to synchronize documentation.
