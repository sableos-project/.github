# SableOS requirements index

Status: **current entry point for implementation, qualification, integration and release work.**

Use this document before source mutation or a build/integration run. It distinguishes current normative requirements from historical milestone/evidence documents so new work does not reconstruct architecture from chat history or obsolete READMEs.

## 1. Required reading protocol

Before changing source, product composition, build tooling or device state:

1. read the organization current roadmap;
2. read the owning repository's architecture/requirements;
3. identify whether the work belongs to standalone application qualification or SableOS product integration;
4. bind the exact source/upstream/artifact baseline;
5. identify permissions, network, privilege, exported-component and data-ownership changes;
6. identify what remains `TBD` and do not let implementation silently choose it;
7. state the validation/authorization boundary before state-changing work.

Requirements define intended behavior. Evidence records whether the requirement was met; do not rewrite old evidence to match a later architecture.

## 2. Organization-wide current documents

### Current development/release plan

```text
sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md
```

Defines the current R8 consolidated train, the Process A / Process B split, integration-freeze requirements, the `ai-g732` builder transition and the one-image/one-device-campaign budget.

### Application reuse/integration architecture

```text
sableos-project/.github/docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md
sableos-project/platform_sable/docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md
```

The organization document defines program policy; the `platform_sable` document defines the shared application/platform architecture. They must agree on workstream scope and claim boundaries.

### Rust/Kotlin architecture

```text
sableos-project/.github/docs/RUST_APPLICATION_ARCHITECTURE.md
```

Governing rule:

```text
RUST_BY_RISK, NOT_RUST_BY_BRANDING
```

Rust owns deterministic/high-value domain logic where useful; Kotlin/Android owns lifecycle, permissions, accessibility, framework/provider APIs, Media3/MediaSession, CameraX, Readium Android integration and other Android-specific behavior. JNI/FFI is a narrow tested boundary, not a goal by itself.

### Default application/replacement policy

```text
sableos-project/.github/docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md
sableos-project/vendor_sable/docs/DEFAULT_APPLICATION_COMPOSITION.md
```

A standalone-qualified Sable APK is not automatically a shipping/default app. Product adoption requires explicit composition/integration/runtime evidence and replacement/rollback planning where applicable.

### CI/build/device/signing trust architecture

```text
sableos-project/.github/docs/CI_TRUST_ARCHITECTURE.md
sableos-project/build/docs/CI_EXECUTION_MODEL.md
```

Current transition:

```text
GitHub hosted     = disposable standalone application/static/security CI
ai-g732           = intended sable-builder-01 after storage/build migration gate
thinkpad-p50      = legacy/reference builder and historical evidence source
Pixel 7 / panther = sable-device-01
OptiPlex          = sable-signer-01
```

Do not run untrusted PR code on the trusted Android builder or signer.

## 3. R5/R6 historical foundation

The following remain important source/evidence history, but they do not define the current forward milestone:

```text
packages_apps_SableStart/docs/MIGRATION_STATUS.md
packages_apps_SableStart/docs/R6_ALL_APPS_AND_GREETING.md
platform_manifest/docs/R5_R3_RECONSTRUCTION_PLAN.md
build/docs/MILESTONE_EVIDENCE_GATES.md   # R5/R6 sections
```

Preserve their original evidence/requirements. Current README/status documents should identify them as historical foundations rather than saying R6 is still the next feature milestone.

## 4. R7 — Panther/product/daily-driver baseline

### Panther runtime matrix

```text
sableos-project/device_sable_panther/docs/R7_DAILY_DRIVER_VALIDATION.md
```

The matrix remains the requirements baseline for calls, contacts, SMS/MMS, Wi-Fi, cellular, browser/Internet, notifications, Settings, camera/photos, files, clock/alarm, Calculator baseline and Sable Start accessibility.

Do not infer unexecuted runtime cases as PASS because R8 source work has started.

### Product-wiring / build evidence model

```text
sableos-project/build/docs/ANDROID_PRODUCT_BUILD_PLAYBOOK.md
sableos-project/build/docs/MILESTONE_EVIDENCE_GATES.md
```

Current R7 forensics reinforce the required claim ladder:

```text
source/module
 -> graph edge
 -> product selection
 -> PRODUCT_OUT
 -> installed-file/target-files
 -> image
 -> runtime
```

Firmware/product packaging work established direct standalone Panther firmware source/product/target-files provenance for ABL/bootloader/radio. That does not remove the need for separate app/runtime qualification.

## 5. R8 — ACTIVE consolidated train

R8 is not theme-only. It combines a shared Sable design foundation with independently qualified application workstreams, followed by one deliberate product integration tranche.

### R8-A — shared design

```text
sableos-project/platform_sable/docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md
```

Authoritative first-R8 behavior:

```text
Follow system
Light
Dark
bounded accent
reset/default
shared semantic tokens
accessibility/readability rules
```

Do not expand first R8 into icon packs, grid/density editors, corner-style editors, theme stores/marketplaces or wallpaper editors without a requirements change.

### Sable Start R8 consumer

```text
sableos-project/packages_apps_SableStart
```

The existing R8 customization PR must be reconciled with the authoritative R8-A contract before merge. Metro/Graphite/OLED and user-selectable corner styles are not currently normative first-R8 requirements.

### R8-B — Calculator + Convert

Current architecture:

- interaction-neutral exact arithmetic/domain primitives may be implemented and tested;
- conversion logic may reuse/refactor portable Rustmix Wave domain logic;
- unresolved Calculator interaction semantics remain requirements `TBD` before they become shipping behavior;
- Android/Compose owns presentation/input;
- core operation requires no network/sensitive permission.

The historical filenames `R9_SABLE_UTILITY_APP_MODEL.md` and `R9_CALCULATOR_REQUIREMENTS_DRAFT.md` contain useful policy/requirements material, but their original milestone assignment is superseded: Calculator/Convert now belong to R8-B.

### R8-C — Games

Initial set:

```text
Sudoku
Minesweeper
2048
```

Deterministic Rust cores where useful; Compose/Android input/presentation; no Lua runtime.

### R8-D — Reader publication path

Primary source:

```text
vaachak-platform/vaachak-mobile
initial qualification pin: 5393503ec0695e87e0a9bc4567fec0fea110ea4d
```

Use Readium/Android reader architecture for EPUB/publication behavior. Do not create a second Android EPUB renderer simply to move logic into Rust.

### R8-D2 — Reader text/accessibility path

Primary source:

```text
vaachak-platform/vaachak-textreader
initial qualification pin: 50fca365baae9869264716569830690fb62029a7
```

Qualified capability target includes TXT, Android share/process-text, TTS/audio export and OCR. It is a capability source for the same Sable Reader product, not a second Sable Reader app.

Upstream Internet permission/model-download behavior is explicit. Strict network-free Reader closure must be decided/proven separately from on-device OCR/translation claims.

Normative supplement:

```text
sableos-project/platform_sable/docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md
```

### R8-E — Media

Local Music + Internet Radio. Portable station/parser/probe logic may be reused from the ESP32 assistant, but Android owns Media3/MediaSession/codec playback, SAF/MediaStore, audio focus/routing, lifecycle/background behavior and networking.

## 6. R8 Process A — standalone qualification

Current standalone qualification work is staged outside the Panther image build and is intentionally split into independent CI lanes:

```text
Rust correctness
Rust dependency/security
Android compile/tests
Android static analysis
Vaachak Reader compile/tests
Vaachak Reader policy/static
Vaachak Text Reader compile/tests
Vaachak Text Reader policy/static
APK artifact seal
repository/policy checks
```

The current staging workspace is `aimindseye/sableos` PR #7. It is a qualification workspace, not the final product source-composition authority.

The integration freeze must bind source commit(s), workflow/run identity, package ID/version, manifest permissions/components, ABI/native-library inventory where relevant, dependency/provenance inventory and APK SHA-256.

## 7. R8 Process B — product integration

Read:

```text
vendor_sable/docs/DEFAULT_APPLICATION_COMPOSITION.md
build/docs/ANDROID_PRODUCT_BUILD_PLAYBOOK.md
platform_manifest/docs/DEVELOPMENT_MILESTONE_COMPOSITION.md
platform_manifest/docs/RELEASE_MANIFEST_POLICY.md
```

The exact Android 17 / GrapheneOS mechanism for consuming sealed standalone APKs must be proved before it becomes normative. `android_app_import` is a candidate only.

Required proof ladder:

```text
sealed APK/source identity
 -> declared Sable product module/import
 -> selected product package
 -> PRODUCT_OUT install identity
 -> installed-file / target-files identity
 -> image membership
 -> runtime package/component identity
 -> user-visible behavior
```

Do not rebuild the Panther image merely to discover that the standalone app does not compile.

## 8. Trusted R8 image builder transition

The next normal Panther image build is planned on `ai-g732` after the 4 TB storage/build migration is itself validated.

Before that build, prove source/repository identity, filesystem/storage identity, host/toolchain prerequisites, OUT/evidence boundaries, target product/release/variant/Build ID, and absence of accidental old-workspace-only inputs.

The ThinkPad P50 remains historical/reference evidence during the transition; no new R8 full image should be scheduled there merely because older output exists.

## 9. R9+ — future coherent tranche

R9 is no longer "first Calculator". Candidate work includes Notes, Voice Notes, Flashcards, selected sensor games, Calendar after provider/data/permission design, future Sable Study/PDF workflow and selected Sable Start improvements.

The same rule applies:

```text
standalone qualification
 -> exact integration freeze
 -> bounded product wiring
 -> one coherent image build
 -> one device campaign
```

## 10. Release identity

Read:

```text
platform_sable/docs/RELEASE_MODEL.md
platform_sable/docs/DEVICE_SUPPORT_LEVELS.md
platform_manifest/docs/RELEASE_MANIFEST_POLICY.md
platform_manifest/docs/SOURCE_COMPOSITION_MODEL.md
build/docs/REPRODUCIBILITY.md
build/docs/AUTHORIZATION_MODEL.md
```

A release/build identity must distinguish:

- exact OS source composition;
- exact external qualified application artifacts when used;
- build/toolchain/host identity;
- target/device/support level;
- artifact hashes;
- signing/update channel;
- runtime qualification and known limitations.

## 11. Documentation status

Use:

```text
sableos-project/.github/docs/DOCUMENTATION_STATUS.md
```

for the current audit classification of organization documentation into current normative, active-workstream, and historical/evidence documents.

## 12. Anti-drift checklist

Before implementation/build work answer:

```text
Which current requirement owns this change?
Which repository/layer owns it?
Process A or Process B?
What is still TBD?
What privilege/network/data authority changes?
What exact source/artifact identity is tested?
What standalone evidence is required?
What product/image evidence is required?
What is explicitly not being claimed?
```

If the answer is absent from current documentation, update the architecture/requirements before encoding the choice in source.