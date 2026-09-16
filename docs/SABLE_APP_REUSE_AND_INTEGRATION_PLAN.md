# SableOS application reuse and integration plan

Status: **normative product direction for application reuse, pre-build verification, and integration-build budgeting.**

This document records how SableOS will reuse existing application/domain code from projects already owned by the same developer while avoiding unnecessary rewrites, duplicate renderers, embedded-runtime baggage, and excessive Panther product rebuilds.

It complements:

- `docs/DEVELOPMENT_RELEASE_PLAN.md`;
- `docs/RUST_APPLICATION_ARCHITECTURE.md`;
- `docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md`;
- application-specific requirements in the owning repositories.

The governing rules are:

```text
REUSE_PROVEN_CODE_BEFORE_REWRITING
HOST_VERIFY_BEFORE_PRODUCT_BUILD
ONE_INTEGRATION_BUILD_PER_COHERENT_TRANCHE
ANDROID_OWNS_ANDROID_INTEGRATION
RUST_OWNS_PORTABLE_DOMAIN_LOGIC_WHERE_IT_MATERIALLY_HELPS
NO_LUA_RUNTIME_IN_SABLEOS_APPLICATIONS
```

## 1. Current source projects considered for reuse

The following repositories are approved as first-party reuse sources for planning because they are owned by the same developer. The listed revisions are research/reference pins observed when this plan was written; any production extraction must bind to the exact source revision actually imported.

| Project | Reference revision | Planned SableOS value |
| --- | --- | --- |
| `aimindseye/rustmix-wave` | `6feeeb4f5941bf9b899033f713dcc5f2987e8bad` | native Rust games, converter, dictionary/domain logic, additional offline utilities |
| `aimindseye/rustmix-x4-firmware` | `46a169e42234eeedf1736974b80f1d34bc63a6cd` | reader/domain concepts, persistence models, bounded embedded implementations useful as reference |
| `aimindseye/ESP32-S3-Touch-LCD-1.85C-Assistant` | `c247b208f2a64921cc5b99b516c8a09234a76f50` | local music and Internet-radio domain/state/parsing logic |
| `vaachak-platform/vaachak-mobile` | `5393503ec0695e87e0a9bc4567fec0fea110ea4d` | production Android/Compose/Readium e-reader implementation and study-oriented foundations |

First-party repository ownership does **not** automatically clear third-party code or assets. Every imported crate/library/font/dictionary/content pack/media codec/data set retains its own license, security-update, and provenance requirements.

## 2. Application-development model

New Sable applications should be developed primarily as ordinary application projects with fast host/Gradle/Cargo validation. The Panther AOSP/Graphene-derived product graph is an integration environment, not the everyday application compiler.

Preferred flow:

```text
GitHub source
    |
    +--> Rust host gates
    |       rustfmt
    |       clippy
    |       cargo test/property tests where applicable
    |
    +--> Kotlin/Gradle host gates
    |       unit tests
    |       static analysis
    |       standalone APK build
    |
    +--> application runtime tests
            emulator / supported Android device / e-ink device as relevant
                |
                v
        freeze exact source + qualified artifact
                |
                v
        SableOS product integration
                |
                v
        ONE normal Panther integration build
                |
                v
        artifact fidelity + Device1 campaign
```

A narrow Soong build is still appropriate when a change must prove Android resource linking, platform API typing, privileged integration, JNI linkage, product-owned framework APIs, or another property that standalone application tests cannot prove. It is not an automatic gate after every source change.

## 3. Product-build budget

A development milestone does not automatically imply a separate full Panther build.

The default policy is:

```text
pure/domain source iteration       -> host tests only
ordinary Android app iteration     -> Gradle/app tests + standalone APK
platform/JNI integration change    -> targeted narrow Android build when justified
coherent application tranche done  -> one normal Panther product integration build
release/device qualification       -> one exact artifact/device evidence campaign
```

Before launching any expensive product target, capture a dry-run or equivalent dependency estimate when practical. Targets such as `target-files-package` must be treated as potentially near-full product builds rather than assumed to be cheap packaging steps.

Repeated direct-Ninja environment repair is not a development strategy. Once an integration checkpoint requires broad product closure, prefer the normal known-good product build path and make that build carry a meaningful feature tranche.

## 4. R8 consolidated application foundation

R8 is expanded from a theme-only milestone into the first coherent Sable native-application foundation.

R8 workstreams may progress and close source-level gates independently. They converge at one integration freeze.

### 4.1 Shared Sable foundation

R8 establishes:

- shared semantic colors, typography, spacing, shape, icon, surface, motion, and accessibility contracts;
- Follow system / Light / Dark;
- bounded accent selection;
- typed/persisted Sable-owned appearance settings;
- Compose UI-test infrastructure;
- AndroidX UIAutomator system-boundary infrastructure;
- Rust host-verification conventions;
- Kotlin/Gradle pre-build verification conventions;
- exact source/artifact provenance rules for standalone app integration.

Sable Start becomes one consumer of the shared design contract rather than the sole owner of it.

### 4.2 Sable Calculator + Convert

Initial direction:

- dedicated Sable Calculator Android application;
- Kotlin/Compose UI;
- deterministic arithmetic/domain core, using Rust where it provides a clean reusable/testable boundary;
- unit conversion as a secondary surface in the same application unless later product requirements justify a separate app;
- reuse/refactor the hardware-independent fixed-point conversion concepts from `rustmix-wave`;
- no network permission for core Calculator/Convert functionality;
- deterministic host tests before Android product integration.

The first release need not become a scientific/programmer calculator merely because the architecture can support it.

### 4.3 Sable Games

Initial R8 game set:

- Sudoku;
- Minesweeper;
- 2048.

Primary reuse source: `aimindseye/rustmix-wave`.

Architecture:

```text
Compose UI / Android input
        |
        | narrow typed JNI boundary where useful
        v
sable-games-core
        +-- sudoku
        +-- minesweeper
        +-- game_2048
```

Reuse the game rules/state/algorithms. Refactor out e-paper rendering, ESP input, firmware storage assumptions, and runtime-specific UI code.

The initial Android versions should use ordinary touch/swipe input. Tilt Maze and Sokoban/Tilt may be considered later with Android sensor adapters, but Rust game cores must remain unaware of `SensorManager` and Android JNI objects.

**Lua is not part of the Sable Games architecture.** Do not port the Rustmix Lua VM, Lua manifests/catalog, Lua application loader, or dynamic Lua game scripting into SableOS.

Prefer one `Sable Games` APK for the initial collection instead of one APK per small game. This gives one manifest, one shared design surface, one test harness, and one integration artifact.

### 4.4 Sable Reader

Primary implementation source: `vaachak-platform/vaachak-mobile`.

Sable Reader should reuse the working Android/Compose/Readium implementation rather than building a second Android EPUB renderer from the embedded Rustmix reader.

Initial R8 scope:

- EPUB;
- TXT;
- library/recent books;
- reading progress;
- bookmarks;
- highlights;
- table of contents;
- in-book search;
- TTS where the existing implementation remains suitable;
- reader appearance controls;
- e-ink-oriented mode/behavior where useful;
- local/offline-first defaults.

Preferred code-ownership model is a Sable product flavor or equivalent shared-source arrangement in Vaachak rather than copying a large reader implementation into a divergent Sable-only fork without reason.

The Sable flavor may use a Sable application ID/branding/theme adapter and a narrower product feature policy while continuing to share the proven reader/core code.

The Rustmix X4 reader remains valuable for domain concepts, compact persistence formats, host-test ideas, and reusable non-rendering logic. It is not the preferred Android EPUB rendering engine.

### 4.5 PDF policy

R8 does **not** embed PDF rendering into Sable Reader.

Initial policy:

```text
Sable Reader     -> EPUB + TXT leisure/book reading
existing secure PDF Viewer -> PDF viewing
future Sable Study -> only if richer PDF annotation/study workflows justify it
```

Sable Reader may later recognize a PDF in a library and delegate it to the installed `application/pdf` handler, but it must not duplicate a mature PDF renderer merely for visual unification.

A future **Sable Study** application may be evaluated for PDF highlighting, annotation, notes, study sessions, organization, search, or related workflows. That is a separate product decision and not an R8 requirement.

### 4.6 Sable Dictionary

Candidate sources:

- native Rust dictionary/lookup work from `rustmix-wave`;
- existing Vaachak dictionary interfaces/providers.

Goal:

- offline-first exact/prefix lookup;
- reusable by Sable Reader and potentially other Sable apps;
- avoid shipping duplicate dictionary engines/data sets when one shared product design can serve both;
- treat dictionary data licensing separately from application-code ownership.

Dictionary may ship in R8 if reuse is straightforward and low-risk; it must not delay the R8 integration freeze if the data/provenance or shared-service design is not ready.

### 4.7 Sable Media: Music + Internet Radio

Primary reuse source: `aimindseye/ESP32-S3-Touch-LCD-1.85C-Assistant`.

The useful reusable portions are domain/state/parsing behavior such as:

- local track discovery/selection concepts;
- WAV/MP3 metadata/probing logic where portable and worthwhile;
- playback-state models;
- Internet-radio station models;
- station-list parsing;
- M3U/M3U8 or simple station-list conventions;
- URL/selection/control state.

Do **not** port the embedded playback backend:

- FreeRTOS tasks;
- PSRAM stream buffers;
- PCM5101/I2S ownership;
- ESP-IDF audio shims;
- HELIX C decoder glue solely because the firmware used it;
- fixed SD-card paths;
- watch/display-specific rendering/input code.

Android should own:

- audio decoding/playback through the supported Android media stack;
- MediaSession/background playback;
- audio focus;
- Bluetooth/headset routing;
- lock-screen/media controls;
- lifecycle/foreground-service behavior;
- MediaStore/Storage Access Framework integration;
- network permission and connectivity behavior for radio.

Sable Media can expose both **Music** and **Internet Radio** in one application because they share playback controls and media-session ownership.

Local Music should not require broad storage privilege when MediaStore/SAF can satisfy the use case. Internet Radio explicitly requires network access and must keep that permission boundary visible in requirements and tests.

## 5. Reuse classification

The default classification for source from the four projects is:

| Source area | Reuse direction |
| --- | --- |
| deterministic Rust game rules/state | **extract/refactor and reuse** |
| Rust unit-conversion logic | **reuse/refactor** |
| Rust dictionary parsing/lookup | **reuse/refactor** |
| Rust reader persistence/domain models | **reuse selectively / reference** |
| Vaachak Readium integration | **reuse substantially** |
| Vaachak Compose reader UI/ViewModel | **reuse substantially, Sable product adaptation** |
| Vaachak e-ink behavior | **reuse selectively; replace device-brand hacks with platform abstraction when needed** |
| ESP radio station parser/state | **extract/refactor and reuse** |
| ESP music domain/state logic | **extract/refactor selectively** |
| ESP/LVGL/e-paper/raw framebuffer UI | **do not port** |
| ESP GPIO/I2S/FreeRTOS/PSRAM-specific code | **do not port** |
| HELIX/firmware decoder glue | **do not port by default** |
| Rustmix Lua runtime/catalog/manifests | **do not port** |
| fixed SD-card paths/embedded storage assumptions | **replace with Android storage adapters** |

## 6. Android/Rust ownership boundary

For reusable hybrid applications, prefer:

```text
Android / Kotlin / Compose
    presentation
    accessibility
    permissions
    lifecycle
    SAF / MediaStore
    sensors
    microphone/audio services
    notifications
    MediaSession
    Readium Android integration
            |
            | narrow typed interface
            v
Rust domain core
    algorithms
    game rules
    deterministic state machines
    parsers
    bounded import/export formats
    validation
    selected persistence transformations
```

Rust code should not receive Android framework objects when a simple typed value/file/byte abstraction can preserve host-testability.

Do not add JNI merely to increase Rust percentage. Follow `docs/RUST_APPLICATION_ARCHITECTURE.md`.

## 7. Pre-build verification gates

### 7.1 Rust gate

Applicable Cargo-managed reusable cores should pass, as appropriate:

```text
cargo fmt --check
cargo clippy -- reviewed lint policy
cargo test
property/fuzz tests for parsers and state machines where justified
unsafe inventory/review
cargo-audit / cargo-deny / provenance checks when the canonical dependency graph supports them
```

Host tests should include deterministic vectors for Calculator, Convert, game rules, media-list/station parsing, dictionary parsing, migrations, and other pure logic.

### 7.2 Kotlin/Gradle gate

Standalone Android applications should pass, as applicable:

```text
Kotlin/JVM unit tests
coroutine/state-flow tests
static analysis/lint
Compose/unit semantics tests
standalone debug/release-like APK build appropriate to the development stage
```

Vaachak-derived code should reuse its existing Gradle/unit/runtime test investment rather than forcing every iteration through Soong.

### 7.3 Android integration gate

Use narrow AOSP/Soong compilation only when needed to prove:

- framework API availability;
- AAPT/resource integration;
- platform signing/privilege assumptions;
- JNI installation/linkage;
- product-owned framework APIs;
- manifest/product composition constraints that standalone Gradle cannot prove.

### 7.4 Full integration gate

The R8 Panther product build starts only after the selected R8 workstreams reach an explicit integration freeze.

The build binds exact source/artifact identities for the included applications and produces the normal product/target-files/image chain. Device qualification then validates cross-app behavior, permissions, defaults, media/storage interactions, theme integration, and regressions.

## 8. Application artifact integration

SableOS should be able to consume exact, qualified standalone application artifacts where that is safer and faster than reproducing a large external Gradle/Maven dependency graph inside Soong.

The exact Android 17/GrapheneOS integration mechanism must be verified in the current tree before it becomes normative. `android_app_import` or another canonical prebuilt-app mechanism may be suitable, but this document does not assume semantics that have not been validated.

Whichever mechanism is selected must record:

- source repository and exact commit;
- reproducible build instructions/toolchain identity;
- APK SHA-256;
- package/application ID;
- signing identity/model;
- min/target/compile SDK constraints;
- permissions/components;
- native library ABI contents when present;
- product partition/install path;
- update/rollback model;
- third-party dependency/provenance evidence.

Do not place opaque manually-built APKs into `vendor_sable` without provenance and rebuildability.

## 9. R8 integration acceptance direction

R8 source workstreams do not each require a separate Panther image.

The intended closure is:

```text
Shared Sable foundation PASS
Calculator/Convert host + app gates PASS
Games host + app gates PASS
Reader host + app gates PASS
Media host + app gates PASS
optional Dictionary gate PASS or explicitly deferred
        |
        v
R8 integration freeze
        |
        v
ONE normal Panther full build
        |
        v
artifact/package fidelity
        |
        v
ONE Device1 integration campaign
```

A workstream may be explicitly deferred rather than forcing a poor-quality implementation merely to preserve a list. The integration freeze must record exactly what is included.

## 10. R9 direction

R9 becomes the **next coherent application/productivity tranche**, not simply "the next single app after Calculator."

Candidates include:

- Notes;
- Flashcards using native Rust/Kotlin rather than Lua runtime execution;
- Voice Notes using Android audio capture/playback with reusable domain logic;
- Tilt Maze / Sokoban using Android sensor adapters;
- Calendar only after local/provider ownership and permission policy is explicitly designed;
- selected Sable Start improvements;
- a future Sable Study PDF workflow if requirements justify it.

R9 should again batch enough host-qualified value to justify one expensive product integration build.

## 11. Explicit non-goals

This plan does not authorize:

- replacing Phone, Messaging, Camera, Browser, SystemUI, Keyguard, or core Settings for branding consistency;
- a Sable browser engine;
- a new Sable PDF renderer in R8;
- a Lua runtime or general scripting platform inside Sable apps;
- porting ESP hardware drivers into Android;
- broad storage/network/sensor permissions merely to preserve firmware behavior;
- full Panther builds after every app feature;
- treating successful host tests as proof of Android runtime integration;
- treating one integration build as proof of every future source revision.

## 12. Decision rule

When evaluating another existing project for SableOS reuse, classify each subsystem as:

```text
REUSE DIRECTLY
EXTRACT / REFACTOR
ANDROID ADAPTER
REFERENCE ONLY
DO NOT PORT
```

Prefer the path that preserves proven behavior while reducing duplicated code, privilege, embedded-specific baggage, and integration-build frequency.

The objective is not to maximize new Sable-owned code. The objective is to create a coherent SableOS application family from already-proven work, with fast host verification and deliberately budgeted product builds.