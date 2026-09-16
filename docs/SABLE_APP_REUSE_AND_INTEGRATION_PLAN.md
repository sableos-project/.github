# SableOS application reuse and integration plan

Status: **normative program policy for R8 application reuse, standalone qualification, artifact freezing and later SableOS integration.**

This document defines the organization-level rules. `platform_sable/docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md` owns the detailed shared application/platform architecture. The two documents must remain consistent.

## 1. Governing rules

```text
REUSE_PROVEN_CODE_BEFORE_REWRITING
QUALIFY_APPS_OUTSIDE_AOSP_FIRST
ANDROID_OWNS_ANDROID_INTEGRATION
RUST_OWNS_PORTABLE_DOMAIN_LOGIC_WHERE_IT_HELPS
NARROW_FFI_ONLY
NO_LUA_RUNTIME_IN_SABLE_APPS
FREEZE_EXACT_ARTIFACTS_BEFORE_PRODUCT_INTEGRATION
ONE_PRODUCT_BUILD_PER_COHERENT_FROZEN_TRANCHE
```

A standalone application PASS is not an image-release claim. A product build is not an application unit-test framework.

## 2. Current reuse sources

Initial planning/qualification pins:

| Source | Initial observed/pinned revision | R8 value |
| --- | --- | --- |
| `aimindseye/rustmix-wave` | exact imported/extracted revision must be recorded per workstream | Convert and deterministic game/domain logic |
| `aimindseye/rustmix-x4-firmware` | exact reference revision per extraction | reader/domain/reference concepts only; not primary Android renderer |
| `aimindseye/ESP32-S3-Touch-LCD-1.85C-Assistant` | exact extraction revision per import | station/media parser/state/probe concepts, not embedded playback backend |
| `vaachak-platform/vaachak-mobile` | `5393503ec0695e87e0a9bc4567fec0fea110ea4d` | Readium/Compose EPUB publication path |
| `vaachak-platform/vaachak-textreader` | `50fca365baae9869264716569830690fb62029a7` | TXT/share/process-text/TTS/OCR capability |

First-party ownership does not waive third-party dependency, font, asset, dictionary, codec, model, license or security-update obligations.

## 3. Process A — standalone application qualification

Normal app development happens outside the Panther product graph.

```text
source / pinned upstream
   |
   +-- Rust correctness
   |     cargo fmt --check
   |     cargo clippy -D warnings
   |     cargo test
   |     property/fuzz/unsafe checks where justified
   |
   +-- Rust dependency/security
   |     RustSec/cargo-audit where Cargo is canonical
   |     dependency/license/provenance inventory
   |
   +-- Android/Kotlin
   |     JVM/unit tests
   |     Compose/instrumentation tests as appropriate
   |     Android lint/static analysis
   |     standalone Gradle APK build
   |
   +-- external Reader sources
         exact commit checkout
         deterministic Sable adaptation
         relevant upstream/Sable tests
         lint/policy checks
         APK build

        -> package/component/permission inspection
        -> native ABI/library inventory
        -> APK SHA-256
        -> workflow/run identity
        -> accepted feature-policy record
```

Local Mac/Linux runs are useful preflight. The accepted GitHub qualification run remains the R8 freeze authority unless a later policy changes that explicitly.

## 4. Process B — product integration

After Process A freezes the exact input set:

```text
sealed application artifacts + exact source identities
            |
            v
prove exact Android 17 / GrapheneOS import/module semantics
            |
            v
vendor_sable common product selection
            |
            v
PRODUCT_OUT install proof
            |
            v
installed-files / target-files / image proof
            |
            v
one Panther image build
            |
            v
runtime package/component/behavior proof
```

`android_app_import` is a candidate only. The current target tree must prove the accepted mechanism before the mechanism is documented as normative.

## 5. R8-A — shared Sable foundation

First-R8 design behavior is intentionally bounded:

```text
Follow system
Light
Dark
bounded accent
reset/default
semantic colors/typography/spacing/shapes
accessibility and test conventions
```

Do not broaden first R8 into density/grid/icon-pack/corner-style/wallpaper/theme-marketplace work without a requirements update.

Sable Start and the first additional Sable applications must consume the same contract rather than copy local constants.

## 6. R8-B — Calculator + Convert

Calculator:

- exact/checked arithmetic primitives may be implemented before UI semantics are selected;
- code must not silently choose precedence, repeated-equals, percent, history or user-visible rounding/display policy while those remain TBD;
- no sensitive permission/network requirement for basic operation.

Convert:

- reuse/refactor hardware-independent conversion logic from Rustmix Wave where correct;
- Android owns entry, presentation and lifecycle;
- conversion math remains deterministic and host-testable.

Whether Calculator and Convert remain separate APKs or converge later is a product decision; the current standalone workspace may qualify them independently without making the final launcher/product decision prematurely.

## 7. R8-C — Sable Games

Initial game set:

```text
Sudoku
Minesweeper
2048
```

Reuse portable Rust rules/state where valuable. Android owns rendering, touch, lifecycle and accessibility. Do not port embedded e-paper UI, firmware storage assumptions or the Rustmix Lua runtime/catalog.

One Games APK is the preferred product shape unless later requirements justify separation.

## 8. R8-D — Sable Reader publication capability

Primary source: pinned Vaachak Mobile.

Reuse Readium/Android/Compose reader behavior substantially. Initial publication capability should be qualified for what source/tests/runtime evidence actually prove; do not claim unsupported formats because another Reader-related repository supports them.

Network-backed Vaachak features are not automatically accepted into Sable Reader merely because upstream provides them.

## 9. R8-D2 — Reader text/accessibility capability

Primary source: pinned Vaachak Text Reader.

Initial capability target:

- local `text/plain` ingestion;
- Android share target;
- Android process-text target;
- Android TTS playback;
- bounded WAV/audio export through supported document APIs;
- CameraX/gallery OCR;
- Latin and Devanagari OCR.

This is a capability provider for the **same Sable Reader product**. Do not ship a second Sable-branded Reader merely because qualification happens against a separate upstream APK.

The current upstream Text Reader declares `INTERNET`, and ML Kit translation can acquire models. Therefore qualification must distinguish:

```text
on-device execution
model already present
model acquisition requiring network
strict network-free accepted product mode
```

Translation may be deferred from the first accepted Sable Reader even if TXT/TTS/OCR are accepted.

## 10. PDF policy

R8 does not create a new Sable PDF renderer merely for brand consistency. Continue using an inherited/proven secure PDF viewer unless a future Sable Study/annotation workflow justifies a separate requirements/security program.

## 11. R8-E — Sable Media

Initial product capability:

- local Music;
- Internet Radio.

Reusable ESP-derived domain work may include station models/list parsing and media probing concepts. Do not port FreeRTOS, I2S, PCM5101 ownership, PSRAM stream buffers, HELIX firmware glue or fixed SD-card assumptions.

Android owns Media3/codec playback, MediaSession, audio focus, Bluetooth/headset routing, lifecycle/background service behavior, SAF/MediaStore access and Internet connectivity.

## 12. JNI/native integration gate

A Rust core and a compiling Kotlin shell are not one end-to-end application proof.

For Rust-backed Android apps, qualification must additionally prove as applicable:

```text
Rust domain tests PASS
Android target native library builds PASS
arm64-v8a library identity
x86_64/emulator library identity where supported
JNI signatures match Kotlin declarations
panic/exception/error contract is bounded
APK contains expected native libraries
representative Kotlin -> JNI -> Rust call works
```

Do not add JNI merely to increase Rust usage.

## 13. Artifact freeze contract

Each accepted application input records at least:

```text
source repository
source commit
upstream/reuse source commit where applicable
build workflow/run
build environment/toolchain identity
application/package ID
versionCode/versionName
APK SHA-256
permissions
exported components/intent filters
native ABI/library inventory
third-party dependency/provenance inventory
feature-policy boundary
known limitations
```

The product build consumes this exact sealed input. A later untracked local rebuild is a different input.

## 14. Product-build budget

Default R8 budget:

```text
host/Cargo/Gradle CI          repeat freely
standalone app/device tests   repeat as needed
product wiring proof          bounded
full Panther integration      once per frozen tranche
Device1 campaign              once per accepted image tranche
```

A target-files or packaging target is not presumed cheap. Use build-graph/dry-run evidence before treating a broad target as a low-cost gate.

## 15. Trusted builder transition

The next intended full R8 Panther build is on `ai-g732` after the new storage/build environment is migrated and sealed. The old ThinkPad P50 remains a historical/reference environment during transition.

Before using `ai-g732` for the R8 image:

- seal filesystem/storage/host identity;
- compare/match source repository revisions;
- verify toolchain and host prerequisites;
- define OUT/evidence directories and free-space floor;
- bind exact frozen R8 application inputs;
- prove target/product/release/Build ID;
- prove the selected prebuilt/application-integration mechanism.

## 16. Canonical repository policy

Do not fork/copy Vaachak or create permanent Sable app repositories merely to make the directory structure look final.

During qualification, exact pinned upstream + deterministic Sable adaptation is acceptable. Once sustained Sable-owned implementation diverges or a stable source boundary emerges, create/move to a dedicated `sableos-project` repository and record the transition/provenance.

`vendor_sable` must never become a dumping ground for copied application source or opaque manually generated APKs.

## 17. R8 closure

R8 application-foundation closure requires:

```text
selected Process A workstreams PASS
exact integration inputs frozen
shared design contract aligned across consumers
product import/wiring mechanism proven
one trusted Panther image built from the frozen set
artifact/package fidelity proven
one bounded Device1 integration campaign completed
remaining limitations/deferred work recorded
```

A source workstream can be explicitly deferred rather than lowering the gate.

## 18. R9 direction

R9 is the next coherent productivity/application tranche, not the first Calculator milestone. Candidate work includes Notes, Voice Notes, Flashcards, Calendar after provider/data policy, selected sensor games, Sable Study/PDF workflow and selected Sable Start improvements.

R9 follows the same qualification -> freeze -> product integration -> image -> device pattern.