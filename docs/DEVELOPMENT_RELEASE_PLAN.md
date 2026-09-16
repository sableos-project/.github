# SableOS development release plan

Status: **normative product direction for the current development train.**

This document defines the current milestone order, build/release workflow and ownership boundaries. Historical requirement/evidence documents remain valid records of what earlier gates required or proved; this document defines what work happens next.

The `R*` labels are internal development/validation milestones, not semantic SableOS product versions. Exact build/release identity remains bound to source/manifest revisions, qualified external artifacts where applicable, target/device identity, build configuration, artifact hashes and signing provenance.

## 1. Current program state

The project has crossed several early boundaries:

- **R5/R6 foundation:** Sable Start source migration/build work established a canonical organization-owned launcher source path and a real launcher/product direction.
- **R7 product/build forensics:** Panther product graph work established strong firmware/product packaging provenance and clarified the separation between module discovery, product selection, PRODUCT_OUT, target-files and runtime claims.
- **R8 is active now:** the next integration tranche combines the shared Sable design contract with independently qualified Sable applications, then integrates one frozen set into one Panther image.

R7 daily-driver/runtime requirements remain valid where they have not yet been exercised on an accepted image. Moving source work to R8 does not retroactively turn untested runtime cases into PASS.

## 2. Current development train

```text
R5/R6  migration + real launcher foundation
        historical/established source/build baseline
             |
             v
R7     Panther product wiring + daily-driver qualification
        evidence baseline; remaining runtime gaps stay explicit
             |
             v
R8     shared design + native application foundation
        PROCESS A: standalone qualification
             |
             v
        exact R8 integration freeze
             |
             v
        PROCESS B: Android product integration
             |
             v
        one Panther image build + one Device1 campaign
             |
             v
R9+    next coherent productivity/replacement tranche
```

Do not reintroduce the superseded sequence "R8 theme only -> R9 first Calculator". Calculator/Convert, Games, Reader and Media are now part of the consolidated R8 application train.

## 3. Two independent execution processes

### 3.1 Process A — application qualification

Process A is the normal development loop for independently developed Sable applications and reusable Rust cores. It does **not** require a Panther/AOSP full image build.

Expected flow:

```text
exact source identity
    |
    +-- Rust correctness/security
    |     rustfmt
    |     clippy
    |     unit/property/fuzz tests as appropriate
    |     RustSec/dependency/provenance checks
    |
    +-- Android/Kotlin application qualification
    |     JVM/unit tests
    |     Android lint/static analysis
    |     standalone Gradle APK build
    |     Compose/instrumentation tests as appropriate
    |
    +-- Reader/upstream qualification
          exact upstream commit
          deterministic Sable adaptation
          upstream/Sable tests
          build/lint/policy checks

        -> package/manifest/permission inspection
        -> native ABI inventory when present
        -> APK SHA-256
        -> build workflow/run identity
        -> dependency/provenance inventory
        -> R8 application qualification freeze
```

Local macOS/Linux builds are useful for fast preflight. GitHub-hosted CI is the freeze authority for the standalone application tranche unless a later policy explicitly changes that role.

The Panther/AOSP tree must not be used as a substitute for normal Rust/Kotlin/Gradle correctness testing.

### 3.2 Process B — SableOS product integration

Process B consumes only exact, qualified application inputs and trusted Sable product source.

```text
qualified/frozen application artifacts + source identities
                |
                v
prove Android 17 / GrapheneOS application-integration mechanism
                |
                v
prove product selection and install path
                |
                v
prove target-files/image wiring expectations
                |
                v
one normal Panther integration image build
                |
                v
artifact/package/hash fidelity
                |
                v
one bounded Panther Device1 campaign
```

`android_app_import` is a candidate integration mechanism, not an architectural fact until its exact semantics are proved in the target Android 17/GrapheneOS tree. Do not copy opaque APKs into `vendor_sable` and call that integration.

## 4. R8 workstreams

R8 is one integration train with independently closable source workstreams.

### R8-A — shared Sable design/test foundation

Authoritative baseline:

```text
Follow system
Light
Dark
bounded accent selection
reset/default behavior
semantic design roles
accessibility/readability rules
```

The first R8 contract does **not** include a general theme marketplace, icon packs, grid/density editors, corner-style editors, wallpaper editors or unrelated launcher personalization.

Sable Start is one consumer of the shared contract, not the sole owner of it.

### R8-B — Sable Calculator + Convert

- deterministic, testable arithmetic/conversion domain logic;
- Kotlin/Compose Android presentation;
- no network or sensitive permission for ordinary Calculator/Convert operation;
- unresolved calculator interaction behavior (precedence, repeated-equals, percent, history, display/rounding policy, etc.) remains a requirements decision;
- domain primitives may exist without silently choosing those product semantics.

### R8-C — Sable Games

Initial set:

```text
Sudoku
Minesweeper
2048
```

Rust owns deterministic rules/state where useful. Android owns rendering, lifecycle, touch/accessibility and other platform behavior. Embedded display/runtime/Lua architecture is not part of Sable Games.

### R8-D — Sable Reader publication path

Primary reuse source: `vaachak-platform/vaachak-mobile`, pinned to an exact accepted upstream revision for each qualification run.

Direction:

- retain/reuse Readium/Android publication handling rather than creating a second EPUB engine;
- qualify EPUB/library/progress/bookmark/highlight/search/TTS/reader-preference behavior independently;
- keep network-backed upstream surfaces outside the accepted Sable product unless explicitly approved.

### R8-D2 — Sable Reader text/accessibility path

Primary reuse source: `vaachak-platform/vaachak-textreader`, initially pinned to:

```text
50fca365baae9869264716569830690fb62029a7
```

Useful capability includes:

- local TXT ingestion;
- Android `ACTION_SEND` and `ACTION_PROCESS_TEXT`;
- Android TTS playback and bounded audio export;
- CameraX/gallery OCR;
- Latin/Devanagari OCR.

This is a capability source for **one Sable Reader product**, not authorization for a second competing Sable Reader launcher entry.

The upstream Text Reader currently declares Internet access and translation can trigger ML Kit model acquisition. Therefore "on-device translation" must not be conflated with "strict network-free operation". Translation/model acquisition is a separate product/privacy gate.

### R8-E — Sable Media

Sable Media initially combines:

- local Music;
- Internet Radio.

Reuse portable domain/state/station-list/probing concepts from the ESP32 assistant where valuable. Android owns Media3/codec playback, MediaSession, audio focus, routing, lifecycle/background playback, storage/document access and Internet connectivity.

Local music should use supported Android user-granted media/document access rather than broad filesystem authority. Internet Radio's network permission is explicit and belongs to Media, not unrelated offline apps.

## 5. Repository/ownership direction

- `.github` owns organization-wide current-state/roadmap/trust/policy documents.
- `platform_sable` owns shared semantic/design/application architecture contracts.
- substantial application source belongs in an application-owned repository/workspace, not `platform_sable`, `vendor_sable` or a device repo for convenience;
- `vendor_sable` owns common product inclusion/integration of qualified applications;
- `device_sable_*` owns only genuine device-specific adaptation/qualification;
- `platform_manifest` owns exact OS source composition and must also bind qualified non-source application inputs through explicit provenance when the product consumes sealed APKs;
- `build` owns trusted Android build/reconstruction/evidence tooling.

Do not create a permanent application repository merely to get ahead of an unstable ownership boundary. Once a Sable application's source/product contract is stable, move it into an appropriately named canonical organization repository and record that transition.

## 6. Product/default-application policy

Daily-driver readiness does not require Sable-owned replacements for every application.

Retain/provision proven inherited implementations for high-integration areas such as Phone, Messaging, Browser and Camera until a separately documented replacement case exists.

A standalone qualified Sable APK is **not automatically a shipping/default application**. Product adoption additionally requires:

- source/artifact provenance;
- package/permission/component review;
- product-integration proof;
- image/runtime proof;
- replacement cleanup/rollback plan when displacing an inherited app.

## 7. Build-host and trust transition

The next R8 Panther product/image build is planned for **`ai-g732`** after the build environment is moved to the expanded storage and a migration preflight seals the source/tool/output environment.

Current transition model:

```text
GitHub hosted     = disposable app/static/security qualification
ai-g732           = intended sable-builder-01 for R8 Android/product builds
thinkpad-p50      = legacy/reference builder and historical evidence source
Pixel 7 / panther = sable-device-01
OptiPlex          = sable-signer-01
```

Before the first R8 image build on `ai-g732`, prove at least:

- filesystem/storage/mount identity and adequate free-space margin;
- transferred source/repository identities;
- toolchain/host prerequisites;
- intended OUT/evidence mutation boundaries;
- exact R8 application freeze identities;
- target product/release/variant/Build ID;
- exact product-wiring mechanism for sealed application artifacts;
- no unexpected stale/local-only dependency on the old workspace.

Do not clean/clobber the old build merely because a new builder exists; preserve useful evidence until migration/reconstruction closure makes it unnecessary.

## 8. R8 integration freeze

The R8 integration freeze must record exactly what is included. At minimum for every accepted app/artifact:

```text
source repository
source commit SHA
upstream/reuse source identity where applicable
qualification workflow/run identity
package/application ID
version code/name
APK SHA-256
manifest permission/component inventory
native ABI/library inventory when applicable
dependency/provenance inventory
accepted feature-policy boundary
```

A workstream may be explicitly deferred instead of forcing incomplete work into the image.

## 9. Panther image-build rule

Normal R8 iteration budget:

```text
Rust/static/unit CI                many times
Gradle/app CI                      many times
standalone/emulator/device app test as needed
product-wiring proof               bounded
full Panther image                 once per frozen integration tranche
Device1 integration campaign       once per accepted image tranche
```

Do not use a broad target such as `target-files-package` under the assumption that it is cheap; dry-run/dependency evidence should determine whether it is effectively a near-full build.

## 10. R9 direction

R9 is the **next coherent productivity/application tranche**, not "first Calculator".

Potential candidates include Notes, Voice Notes, Flashcards, selected sensor-based games, Calendar after provider/permission policy is explicit, future Sable Study/PDF workflows, and selected Sable Start improvements.

The selected R9 set must be documented before its integration freeze. Candidate status is not implementation authorization.

## 11. Release model

A development milestone is not a public release.

A release claim requires:

- exact complete source composition;
- exact qualified external artifact inputs when present;
- build/toolchain identity;
- supported target/device identity;
- artifact hashes;
- runtime qualification appropriate to the support level;
- signing/update provenance;
- known limitations.

Historical manifests/evidence remain immutable records even after the current product moves on.

## 12. Change-control / anti-drift rules

Before implementation or integration, answer:

```text
What requirement is being satisfied?
Which repository/layer owns it?
Is there proven code we should reuse rather than rewrite?
Does this choose a still-TBD product semantic?
Does it add authority, permission, network use or exported components?
Can it be qualified outside the product build?
What exact evidence closes the standalone app claim?
What exact evidence closes the product/image claim?
What rollback/fallback exists?
```

If an answer is missing, update the requirements/architecture before silently encoding the decision in source or build files.

Compilation is necessary evidence. It is never sufficient evidence for product inclusion, runtime correctness or release closure.