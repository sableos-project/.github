# SableOS requirements index

Status: **current entry point for implementation, qualification, integration and later release work.**

Use this document before source mutation or a build/integration run. It distinguishes current normative requirements from historical milestone/evidence documents so new work does not reconstruct architecture from chat history or obsolete READMEs.

## 1. Required reading protocol

Before changing source, product composition, build tooling or device state:

1. read the organization development plan;
2. read the owning repository's architecture/requirements;
3. identify the current stage: A1, A2, B1, B2/B3 or later release-signing work;
4. bind exact source/upstream/artifact/toolchain baseline;
5. identify permission, network, privilege, exported-component and data-ownership changes;
6. identify unresolved `TBD` behavior and do not let implementation silently choose it;
7. state validation and authorization boundaries before mutation.

Requirements define intended behavior. Evidence records whether it was met.

## 2. Organization-wide normative documents

### Development/release plan

```text
sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md
```

Defines the consolidated R8 pipeline:

```text
A1 disposable GitHub qualification
 -> A2 trusted standalone app build on ai-g732
 -> exact trusted app freeze
 -> B1 pre-image Android/Soong integration proof
 -> B2 Panther development image/acceptance
 -> B3 Titan 2 portability image/acceptance
 -> later production-signing workstream
```

### Trust architecture

```text
sableos-project/.github/docs/CI_TRUST_ARCHITECTURE.md
sableos-project/build/docs/CI_EXECUTION_MODEL.md
```

Current roles:

```text
GitHub hosted     = disposable/untrusted A1 CI
ai-g732           = intended trusted A2/B1/B2/B3 development builder
thinkpad-p50      = historical/reference builder during migration;
                    future signing-host candidate only
Pixel 7 / panther = primary R8 runtime target
Titan 2           = second R8 portability/runtime target
```

There is no active `sable-signer-01`. OptiPlex is removed from the planned signing architecture. Production signing is deferred until development qualification is satisfactory on Panther and Titan 2.

### Application reuse/Rust-Kotlin architecture

```text
sableos-project/.github/docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md
sableos-project/.github/docs/RUST_APPLICATION_ARCHITECTURE.md
sableos-project/platform_sable/docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md
```

Governing rule:

```text
RUST_BY_RISK, NOT_RUST_BY_BRANDING
```

Rust owns deterministic/high-value domain logic where useful; Kotlin/Android owns lifecycle, permissions, accessibility, framework/provider APIs, Media3/MediaSession, CameraX, Readium Android integration and other platform-facing behavior.

### Default application/replacement policy

```text
sableos-project/.github/docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md
sableos-project/vendor_sable/docs/DEFAULT_APPLICATION_COMPOSITION.md
```

A standalone-qualified APK is not automatically a shipping/default app.

## 3. Historical R5/R6 foundation

Preserve as historical requirements/evidence:

```text
packages_apps_SableStart/docs/MIGRATION_STATUS.md
packages_apps_SableStart/docs/R6_ALL_APPS_AND_GREETING.md
platform_manifest/docs/R5_R3_RECONSTRUCTION_PLAN.md
build/docs/MILESTONE_EVIDENCE_GATES.md   # historical sections
```

Do not rewrite earlier evidence to fit the current architecture.

## 4. R7 evidence baseline

### Panther runtime matrix

```text
sableos-project/device_sable_panther/docs/R7_DAILY_DRIVER_VALIDATION.md
```

R7 remains the baseline for calls, contacts, SMS/MMS, Wi-Fi, cellular, browser/Internet, notifications, Settings, camera/photos, files, clock/alarm, Calculator baseline and Sable Start accessibility. Unexecuted cases remain unproven.

### Build/product claim ladder

```text
source/module
 -> graph edge
 -> product selection
 -> PRODUCT_OUT
 -> target-files
 -> filesystem image
 -> runtime
```

Read:

```text
sableos-project/build/docs/ANDROID_PRODUCT_BUILD_PLAYBOOK.md
sableos-project/build/docs/MILESTONE_EVIDENCE_GATES.md
```

## 5. R8 application workstreams

### R8-A — shared design

```text
sableos-project/platform_sable/docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md
```

Normative first-R8 appearance:

```text
Follow system
Light
Dark
bounded accent
reset/default
shared semantic roles
accessibility/readability rules
```

### R8-B — Calculator + Convert

Deterministic arithmetic/conversion domain behavior, Kotlin/Compose presentation, no network/sensitive permission for core operation. Unresolved Calculator interaction semantics remain requirements `TBD`.

### R8-C — Games

Sudoku, Minesweeper and 2048; deterministic Rust cores where useful; Compose/Android presentation/input; no Lua runtime.

### R8-D — Reader publication path

Primary source: `vaachak-platform/vaachak-mobile` / Readium.

### R8-D2 — Reader text/accessibility path

Primary source: `vaachak-platform/vaachak-textreader`.

Accepted capability target includes TXT, share/process-text, TTS/audio export and OCR. Network/model-download behavior is a separate privacy gate. One Sable Reader product composes D and D2.

Normative supplement:

```text
sableos-project/platform_sable/docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md
```

### R8-E — Media

Local Music + Internet Radio. Android owns Media3/MediaSession, codecs, storage/document access, audio routing, lifecycle/background behavior and networking.

## 6. R8-A1 — disposable qualification

GitHub CI lanes may include:

```text
Rust correctness
Rust dependency/security
Android compile/tests
Android static analysis
Reader compile/tests
Reader policy/static
Text Reader compile/tests
Text Reader policy/static
qualification APK seal
repository/workflow policy
```

A1 output is qualification evidence. It is not automatically the trusted artifact that enters the product.

## 7. R8-A2 — trusted standalone application build

A2 runs on `ai-g732` after builder migration/preflight closure.

For each accepted app bind:

```text
source/upstream commits
Gradle wrapper/version
JDK
SDK/NDK
Rust toolchain + lockfile hashes
trusted APK SHA-256
package/version
permissions/components
classes*.dex hashes
JNI .so hashes
native ABI inventory
16 KiB ELF/APK compatibility
dependency/provenance inventory
```

Where reproducible, compare A1 and A2 outputs. Do not promote unexplained differences into a release input.

## 8. R8 integration freeze

Only exact A2-qualified artifacts enter B1. A workstream may be deferred rather than forcing incomplete work into the image.

## 9. R8-B1 — pre-image integration gate

Read:

```text
sableos-project/build/docs/R8_PREIMAGE_GATE.md
sableos-project/build/gates/r8_app_artifact_audit.sh
sableos-project/vendor_sable/docs/DEFAULT_APPLICATION_COMPOSITION.md
sableos-project/platform_manifest/docs/DEVELOPMENT_MILESTONE_COMPOSITION.md
```

`android_app_import` is the preferred candidate, but exact Android 17/GrapheneOS behavior must be observed.

B1 proves separately:

```text
frozen trusted APK input
 -> Soong import/module processing
 -> signing/certificate mode
 -> JNI handling
 -> dexpreopt / uses-library wiring
 -> product selection
 -> PRODUCT_OUT install
```

Target-files/image/runtime remain later layers.

## 10. Native 16 KiB compatibility

Every R8 APK containing native libraries must pass verified 16 KiB compatibility.

Required evidence includes:

```text
ELF PT_LOAD alignment >= 0x4000
APK ZIP alignment suitable for uncompressed native libraries
runtime page size measured on device
representative JNI execution
```

The requirement is verified output compatibility, not one hard-coded linker flag regardless of NDK/toolchain.

## 11. R8-B2 — Panther

After A1/A2/freeze/B1 close for the selected tranche, build one coherent Panther development image and run one bounded Panther campaign. Do not use the full image as the first compiler for ordinary app code.

## 12. R8-B3 — Titan 2 portability

Where compatible, Titan 2 must consume the same frozen common R8 app artifacts and common `vendor_sable` integration as Panther.

| Dimension | Panther | Titan 2 |
| --- | --- | --- |
| ABI | `arm64-v8a` | `arm64-v8a` |
| Rust target | `aarch64-linux-android` | `aarch64-linux-android` |
| 16 KiB compatibility | required | required |
| common app artifacts | baseline | same where compatible |
| common product composition | `vendor_sable` | `vendor_sable` |
| OUT_DIR | isolated | isolated |
| runtime page size | measured | measured |
| physical keyboard | baseline | explicit gate |
| square display | baseline | explicit gate |
| Reader OCR/TTS | capability gate | capability gate |
| Media3/audio | capability gate | capability gate |

Titan-specific secondary-display/program-key/FM features are not common R8 requirements unless separately approved.

R8 portability closes only with bounded target adapters and no common application source fork.

## 13. Build-host migration

Before using `ai-g732` for A2/B1/B2/B3, prove host/storage/source/tool/output identities, isolated OUT_DIR policy, free-space monitoring, target selection, application-freeze inputs and absence of accidental old-workspace-only dependencies.

## 14. Production signing — later

Production APK keys, AVB hierarchy, OTA signing, `sign_target_files_apks`, key custody/backup/recovery/rotation and signed release handoff are deliberately deferred until Panther and Titan 2 development qualification is satisfactory.

The ThinkPad P50 is only a future signing-host candidate. Do not call it `sable-signer-01` until the role is designed and commissioned.

## 15. R9+

R9 is the next coherent productivity/application tranche, not first Calculator. Candidate work remains Notes, Voice Notes, Flashcards, selected sensor games, Calendar after provider/data/permission design, future Sable Study/PDF workflow and selected Sable Start improvements.

## 16. Anti-drift checklist

Before implementation/build work answer:

```text
Which current requirement owns this change?
Which repository/layer owns it?
A1, A2, B1, B2/B3 or later release work?
What is still TBD?
What privilege/network/data authority changes?
What exact source/artifact/toolchain identity is tested?
What evidence closes this layer?
What later claim remains unproven?
```

If the answer is absent from current documentation, update requirements before encoding the choice in source.
