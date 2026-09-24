# SableOS application reuse and integration plan

> **HISTORICAL R8 PROGRAM POLICY — superseded current-state note (2026-09-24):** Pixel 7 / Panther R9 is physically accepted and frozen. K1/K2 multi-device artifact/deployment foundation is merged. Active work is keyboard-first common-product design plus Titan 2 / Titan 2 Elite research. Current authority: `docs/CURRENT_RELEASE_STATUS.md` and `docs/DEVELOPMENT_RELEASE_PLAN.md`.


Status: **HISTORICAL / SUPERSEDED PROGRAM POLICY — retained for R8 provenance.**

This document defines organization-level rules. `platform_sable/docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md` owns the detailed shared application/platform architecture. The two documents must remain consistent.

## 1. Governing rules

```text
REUSE_PROVEN_CODE_BEFORE_REWRITING
QUALIFY_APPS_OUTSIDE_AOSP_FIRST
REBUILD_ACCEPTED_SOURCE_ON_TRUSTED_BUILDER
ANDROID_OWNS_ANDROID_INTEGRATION
RUST_OWNS_PORTABLE_DOMAIN_LOGIC_WHERE_IT_HELPS
NARROW_FFI_ONLY
NO_LUA_RUNTIME_IN_SABLE_APPS
FREEZE_EXACT_TRUSTED_ARTIFACTS_BEFORE_PRODUCT_INTEGRATION
VERIFY_NATIVE_16K_COMPATIBILITY
ONE_PRODUCT_BUILD_PER_COHERENT_FROZEN_TRANCHE
PORT_COMMON_ARTIFACTS_ACROSS_PANTHER_AND_TITAN2_WHERE_COMPATIBLE
DEFER_PRODUCTION_SIGNING_UNTIL_DUAL_TARGET_DEVELOPMENT_IS_STABLE
```

An A1 standalone PASS is not a trusted product artifact. An Android product build is not an application unit-test framework.

## 2. Current reuse sources

| Source | Initial observed/pinned revision | R8 value |
| --- | --- | --- |
| `aimindseye/rustmix-wave` | exact imported/extracted revision per workstream | Convert and deterministic game/domain logic |
| `aimindseye/rustmix-x4-firmware` | exact reference revision per extraction | reader/domain/reference concepts only |
| `aimindseye/ESP32-S3-Touch-LCD-1.85C-Assistant` | exact extraction revision per import | station/media parser/state/probe concepts only |
| `vaachak-platform/vaachak-mobile` | `5393503ec0695e87e0a9bc4567fec0fea110ea4d` | Readium/Compose publication path |
| `vaachak-platform/vaachak-textreader` | `50fca365baae9869264716569830690fb62029a7` | TXT/share/process-text/TTS/OCR capability |

First-party ownership does not waive third-party dependency, font, asset, dataset, codec, model, license or security-update obligations.

## 3. A1 — disposable standalone qualification

Normal fast app development happens outside the Android product graph.

```text
source / pinned upstream
   |
   +-- Rust correctness/security
   +-- Android/Kotlin compile/tests/lint
   +-- Reader/Text Reader pinned-upstream qualification
   +-- package/component/permission inspection
   +-- native ABI inventory
   +-- qualification APK SHA-256
   +-- workflow/run + dependency provenance
```

Local Mac/Linux runs are useful preflight. GitHub-hosted CI is disposable/untrusted qualification infrastructure. Its artifacts are evidence, not automatically trusted product binaries.

## 4. A2 — trusted standalone application build

`ai-g732` rebuilds accepted source using pinned/recorded toolchains and produces the exact artifact eligible for the R8 freeze.

Record at least:

```text
source/upstream commits
Gradle wrapper/version
JDK
Android SDK/NDK
Rust toolchain + Cargo.lock hash
trusted APK SHA-256
package/version
permissions/components
classes*.dex extracted-content SHA-256
lib/<abi>/*.so extracted-content SHA-256
native ABI inventory
16 KiB ELF compatibility
APK native-library ZIP alignment
dependency/provenance inventory
```

Where reproducible, compare A1 and A2 outputs. An unexplained divergence blocks the trusted freeze until understood.

## 5. Exact R8 application freeze

Only exact A2-qualified artifacts enter Android product integration. A workstream may be explicitly deferred rather than forcing incomplete work into the image.

## 6. B1 — pre-image Android product integration

On `ai-g732`:

```text
trusted frozen APK
 -> discover/query exact Soong/Ninja graph
 -> prove import/module input identity
 -> record certificate/signing behavior
 -> record JNI handling
 -> record dexpreopt/uses-library state
 -> build minimum import dependencies
 -> compare processed DEX/JNI content identities
 -> prove product selection separately
 -> prove PRODUCT_OUT separately
```

`android_app_import` is the preferred candidate, but its exact Android 17 / GrapheneOS behavior must be observed before being normalized.

Outer APK hashes may change legitimately during container/signing processing. Therefore track both whole-file hashes and extracted DEX/JNI identities.

## 7. B2 — Panther development image

After A1/A2/freeze/B1 close for the selected tranche, build one coherent Panther development image and run one bounded Panther runtime/regression campaign.

Do not use the broad image build as the first place ordinary Kotlin/Rust/application defects are discovered.

## 8. B3 — Titan 2 portability image

After Panther acceptance, integrate the same frozen common application artifacts into Titan 2 wherever compatible, using the same common `vendor_sable` product composition and an isolated target OUT_DIR.

The portability objective is:

```text
same qualified common app source/artifacts
+ same common product integration
+ bounded target-specific adapters
+ target-specific runtime acceptance
+ no common application fork
```

Titan 2 adds explicit physical-keyboard/focus/text-input and square-display layout validation. Secondary-display/program-key/FM features are not common R8 requirements unless separately approved.

## 9. R8-A — shared Sable foundation

First-R8 design behavior remains:

```text
Follow system
Light
Dark
bounded accent
reset/default
shared semantic design roles
accessibility/test conventions
```

Do not broaden first R8 into grid/density/icon-pack/corner/theme-marketplace/wallpaper work without a requirements change.

## 10. R8-B — Calculator + Convert

Calculator may implement exact/checked domain primitives without silently choosing still-TBD interaction semantics such as precedence, percent, repeated-equals, history or display-rounding policy.

Convert should reuse/refactor hardware-independent Rustmix Wave conversion logic where correct. Android owns input/presentation/lifecycle.

## 11. R8-C — Games

Initial games: Sudoku, Minesweeper and 2048. Reuse portable deterministic Rust rules/state where useful. Android owns rendering, touch/keyboard input, lifecycle and accessibility. No Lua/e-paper firmware runtime.

## 12. R8-D / D2 — one Sable Reader product

Use Vaachak Mobile / Readium for publication/EPUB behavior and qualify Vaachak Text Reader capabilities for TXT/share/process-text/TTS/OCR.

Do not ship two competing Sable Reader apps merely because qualification happens against two upstreams.

Network/model-download behavior is an explicit policy gate. On-device execution is not equivalent to strict network-free operation.

## 13. R8-E — Sable Media

Local Music + Internet Radio. Portable station/parser/probe concepts may be reused; Android owns Media3/codec playback, MediaSession, audio focus/routing, lifecycle/background behavior, SAF/MediaStore and networking.

## 14. JNI/native integration

For Rust-backed apps prove as applicable:

```text
Rust domain tests PASS
Android target native build PASS
arm64-v8a library identity
JNI signatures match Kotlin declarations
panic/error boundary bounded
APK expected .so inventory
representative Kotlin -> JNI -> Rust execution
```

Do not introduce JNI merely to increase Rust usage.

## 15. Native 16 KiB compatibility

Every R8 APK containing native libraries must be verified compatible with 16 KiB page-size systems.

Evidence includes:

```text
ELF PT_LOAD alignment >= 0x4000
APK ZIP alignment suitable for uncompressed native libraries
runtime page size measured on accepted devices
representative JNI execution
```

Use the pinned NDK/toolchain's appropriate mechanism; do not hard-code one linker flag if the toolchain already emits compliant binaries.

## 16. Common product ownership

Common imported-module definitions and common `PRODUCT_PACKAGES` selection belong in `vendor_sable`.

Panther and Titan 2 products inherit common Sable composition and add only documented target exceptions. Do not duplicate the common app list in every device repository.

## 17. Build budget

```text
A1 CI                               repeat freely
local preflight                     repeat freely
A2 trusted app build                as accepted source changes
B1 product-wiring proof             bounded
B2 Panther full image               once per frozen tranche
Panther device campaign             once per accepted image
B3 Titan 2 portability image        once after Panther acceptance
Titan 2 device campaign             once per accepted portability image
```

A target-files/packaging target is not presumed cheap. Use graph/dry-run evidence first.

## 18. Trusted builder transition

Before A2/B1/B2/B3 on `ai-g732`, seal host/storage/source/tool/output identities, OUT/evidence roots, free-space floor/monitoring, target configuration and exact frozen app inputs. Do not copy ThinkPad-specific absolute path assumptions into generic tooling.

## 19. Production signing — deferred

Development/test signing may be used for functional engineering images.

Production application keys, AVB hierarchy, OTA signing, `sign_target_files_apks`, key custody/backup/recovery/rotation and release artifact handoff are deliberately deferred until Panther and Titan 2 development qualification is satisfactory.

The ThinkPad P50 is a future signing-host candidate only after Android building moves to `ai-g732`. It is not yet `sable-signer-01`. OptiPlex is removed from the current signing plan.

## 20. R8 closure

R8 development architecture closes only when selected workstreams have accepted A2 artifacts, B1 integration proof, Panther development-image/runtime acceptance and Titan 2 portability acceptance within stated boundaries.

This still does not constitute a production signed release claim.

## 21. R9 direction

R9 is the next coherent productivity/application tranche, not the first Calculator milestone. It follows the same A1 -> A2 -> freeze -> B1 -> image -> device evidence model.
