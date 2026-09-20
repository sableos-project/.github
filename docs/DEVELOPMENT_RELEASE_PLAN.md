# SableOS development release plan

> **2026-09-20 current execution overlay:** R8 application/design/product integration
> is no longer the active top-level milestone. **R9 is active**: Launcher3/Quickstep
> HOME migration and Sable Start visual closure are complete, the source-bound fresh
> Panther full build is PASS, physical Pixel 7 qualification is next, and
> Titan 2 and Titan 2 Elite remain queued as complementary keyboard-first portability targets. GitHub-hosted
> build CI is retired; canonical qualification runs locally on the controlled build
> machine. See [CURRENT_RELEASE_STATUS.md](CURRENT_RELEASE_STATUS.md).
>
> Where older R8/A1 hosted-CI sequencing below conflicts with this overlay, treat
> it as preserved architecture/history rather than the current execution order.


Status: **normative product direction for the current development train.**

This document defines the current milestone order, build/release workflow and ownership boundaries. Historical requirement/evidence documents remain valid records of what earlier gates required or proved; this document defines what work happens next.

The `R*` labels are internal development/validation milestones, not semantic SableOS product versions. Exact build/release identity remains bound to source/manifest revisions, qualified external artifacts where applicable, target/device identity, build configuration, artifact hashes and, when production signing is later activated, signing provenance.

## 1. Current program state

- **R5/R6 foundation:** launcher/source migration and early product architecture.
- **R7 product/build forensics:** Panther product graph, target-files and runtime evidence discipline.
- **R8 foundation:** shared design, first-party applications, artifact freeze and product composition.
- **R9 is active now:** Launcher3/Quickstep HOME foundation + Sable Start visual closure + fresh Panther build causality are PASS; physical Pixel 7 runtime acceptance is next.
- **Titan 2:** queued after Panther R9 acceptance as the keyboard-first N0 portability/GSI lab.

R7 daily-driver/runtime requirements remain valid where not yet exercised on an accepted image. Starting R8 does not turn untested R7 runtime cases into PASS.

## 2. Current development train

```text
R8 foundation
  shared design + Calculator/Games/Reader/Media/Hub/Mail + product composition
             |
             v
R9-L
  Launcher3/Quickstep HOME + Sable Start production presentation
  visual gate PASS
             |
             v
R9-P
  local-CI qualified, source-bound fresh Panther full build
             |
             v
R9-D
  physical Pixel 7 HOME / Overview / Recents / runtime acceptance
             |
             v
Titan 2
  T0 stock inventory -> keyboard-first N0 GSI portability qualification
             |
             v
later
  broader product tranche + production signing/OTA release engineering
```

Do not reintroduce the superseded sequence `R8 theme only -> R9 first Calculator`. Calculator/Convert, Games, Reader and Media are part of R8.

## 3. R8 execution stages

### 3.1 R8-A1 — disposable qualification

GitHub-hosted CI is an untrusted/disposable feedback and qualification layer. It may execute pull-request code and normal public dependency resolution under its declared workflow policy, but it must not possess production signing material or direct access to the persistent trusted Android build workspace.

Expected A1 lanes include:

```text
Rust formatting / Clippy / unit-property-fuzz tests
Rust dependency/advisory/provenance checks
Kotlin/JVM unit tests
Gradle Android builds
Android Lint/static analysis
Compose/emulator/instrumentation tests where useful
Reader/Text Reader pinned-upstream qualification
manifest/package/permission/component inspection
native ABI inventory
CI APK SHA-256
workflow/source/dependency provenance
```

A green A1 run proves only its standalone claim. A1 artifacts are qualification evidence, not automatically trusted release/product binaries.

### 3.2 R8-A2 — trusted standalone application build

`ai-g732` is the trusted application-build and Android development-build host after its storage/source/tool/output migration gate passes.

A2 rebuilds the accepted source with pinned/recorded toolchains and produces the application artifacts that can enter the R8 freeze.

Record at least:

```text
source repository + exact commit
upstream/reuse commit(s)
Gradle wrapper/version
JDK
Android SDK/NDK
Rust toolchain + Cargo.lock hash
package/version
permissions/exported components
APK SHA-256
classes*.dex extracted-content SHA-256
lib/<abi>/*.so extracted-content SHA-256
native ABI inventory
16 KiB ELF compatibility
APK native-library ZIP alignment
```

Where reproducible, compare A1 and A2 outputs/hashes; a mismatch is evidence to explain, not an automatic failure if the environments intentionally differ.

The A2 result is the trusted standalone artifact freeze candidate.

### 3.3 Exact R8 application freeze

Only exact A2-qualified inputs enter product integration. At minimum bind:

```text
source/upstream identity
accepted feature/policy boundary
trusted-build toolchain identity
trusted APK SHA-256
DEX/JNI inner-content identities
package/version
permissions/components
native ABI inventory
16 KiB compatibility result
dependency/provenance inventory
```

A workstream may be explicitly deferred instead of forcing incomplete work into the image.

### 3.4 R8-B1 — pre-image Android integration gate

B1 exists to detect Soong/import/product-wiring problems before authorizing a broad image build.

Required sequence:

```text
bind exact target product/release/variant + isolated OUT_DIR
 -> generate Soong graph without broad compilation where supported
 -> discover exact import inputs/intermediate outputs
 -> query android_app_import/module processing
 -> record signing/certificate mode
 -> record JNI handling
 -> record dexpreopt + uses-library configuration
 -> build only the import/minimum dependencies
 -> compare processed APK DEX/JNI content identities
 -> prove product selection separately
 -> prove PRODUCT_OUT installation separately
```

When target-files/images are later generated, their membership and filesystem contents are separate evidence layers.

`android_app_import` is the preferred candidate for standalone Gradle APK integration but is not treated as proven until exact Android 17 / GrapheneOS target-tree behavior is observed.

### 3.5 R8-B2 — Panther development image

After A1/A2/freeze/B1 close for the selected tranche, build one coherent Panther development image and run one bounded Panther campaign.

Do not use the Panther full product build as the first compiler/debugger for ordinary application source.

### 3.6 R8-B3 — Titan 2 portability development image

Titan 2 is the second R8 portability target. Where Android/platform compatibility permits, it should consume the same frozen common R8 application artifacts and common `vendor_sable` integration as Panther.

A successful second boot alone is insufficient; the purpose is to prove the common application/product boundary survives a materially different hardware/input/display substrate without forking common app source.

## 4. R8 application workstreams

### R8-A — shared Sable design/test foundation

```text
Follow system
Light
Dark
bounded accent selection
reset/default behavior
semantic design roles
accessibility/readability rules
```

No first-R8 theme marketplace, icon packs, grid/density editor, user-selectable corner system or wallpaper editor unless requirements explicitly change.

### R8-B — Calculator + Convert

- deterministic, testable arithmetic/conversion domain logic;
- Kotlin/Compose Android presentation;
- no network or sensitive permission for core operation;
- unresolved interaction semantics remain requirements decisions.

### R8-C — Games

Initial set: Sudoku, Minesweeper and 2048. Rust may own deterministic rules/state; Android owns UI/lifecycle/input/accessibility. No Lua runtime.

### R8-D — Reader publication path

Reuse `vaachak-platform/vaachak-mobile` / Readium for EPUB/publication behavior rather than creating a second Android EPUB engine.

### R8-D2 — Reader text/accessibility path

Reuse qualified `vaachak-platform/vaachak-textreader` capability for TXT, share/process-text, TTS and OCR. Compose into one Sable Reader product. Translation/model-download/network behavior remains an explicit privacy gate.

### R8-F — Sable Hub / central messaging baseline

R8-F is now part of the first usable Panther/Titan common-app gate and must close before the next Panther full image.

Product direction:

```text
user-visible surface: Sable Messages
architecture:         Sable Hub
package:              org.sableos.hub

views:
  ALL
  MESSAGES
  PEOPLE
  SERVICES
```

The implementation retains mature Android transport/enforcement while Sable owns conversation/person/service presentation.

Baseline capability:

- SMS conversation/read/compose over supported Android Telephony capability;
- ContactsProvider person identity;
- underlying proven Android messaging transport retained for MMS/RCS until Sable fully qualifies those default-handler responsibilities;
- isolated provider web capsules for WhatsApp, Instagram, Facebook/Messenger and LinkedIn where no appropriate consumer API exists;
- explicit provider class: NATIVE_DATA / ANDROID_NOTIFICATION / SUPPORTED_API / WEB / UNAVAILABLE;
- no private application database/protocol scraping;
- no claim that WEB capability is a native unified inbox;
- one common semantic implementation for Panther touch-first and Titan 2 keyboard-first presentation.

R8-F must not become the default SMS role holder until the complete Android default-SMS responsibilities, including SMS/MMS receive/write behavior and rollback, are independently qualified.

Required pre-image marker:

```text
R8_F_SABLE_HUB_MESSAGES=PASS
```

### R8-E — Media

Local Music + Internet Radio. Android owns Media3/MediaSession, codecs, audio focus/routing, lifecycle/background behavior, storage/document access and networking. Portable parsing/state logic may be reused where valuable.

## 5. Native/JNI portability rule

All R8 APKs containing native libraries must be verified compatible with 16 KiB page-size systems.

The normative requirement is **verified compatibility**, not one hard-coded linker flag. If the pinned NDK/toolchain already emits compliant binaries, verify the result. If it does not, use the required linker configuration and then verify it.

Evidence should include:

```text
ELF PT_LOAD alignment >= 0x4000
APK ZIP alignment suitable for uncompressed native libraries
runtime page size measured on device
representative JNI execution on accepted images
```

Do not infer page size from SoC/vendor identity alone.

## 6. Titan 2 compatibility matrix

| Dimension | Panther | Titan 2 |
| --- | --- | --- |
| Android ABI | `arm64-v8a` | `arm64-v8a` |
| Rust target | `aarch64-linux-android` | `aarch64-linux-android` |
| 16 KiB native compatibility | required | required |
| common frozen app artifacts | baseline | same where compatible |
| common product composition | `vendor_sable` | `vendor_sable` |
| device adapter | Panther-specific | Titan-specific |
| OUT_DIR | isolated | isolated |
| runtime page size | measured | measured |
| touch | validate | validate |
| physical keyboard | baseline | explicit navigation/focus/input gate |
| square display | baseline | explicit layout gate |
| Reader OCR/TTS | capability gate | capability gate |
| Media3/audio | capability gate | capability gate |
| production signing | deferred | deferred |

Titan-specific secondary-display, programmable-key, FM-radio and other vendor capabilities are not R8 common-app requirements unless separately approved.

R8 common-app portability closes only when the same common application source/artifacts and common product integration are accepted on both targets with bounded target adapters and no common app fork.

## 7. Repository/ownership direction

- `.github` — organization roadmap/trust/policy.
- `platform_sable` — shared semantic/design/application architecture.
- application-owned repositories/workspaces — substantial app source/build/test ownership.
- `vendor_sable` — common imported-module definitions and common product selection of qualified applications.
- `device_sable_*` — genuine target-specific adaptation/qualification only.
- `platform_manifest` — exact OS source composition and accepted external artifact provenance.
- `build` — trusted app/image build, reconstruction, pre-image/product-wiring and evidence tooling.

Do not duplicate the common R8 `PRODUCT_PACKAGES` list in each device repository. Device products inherit common Sable composition and add only documented target exceptions.

## 8. Build-host and signing transition

Current roles:

```text
GitHub hosted     = disposable/untrusted A1 CI
ai-g732           = intended trusted A2/B1/B2/B3 development builder
thinkpad-p50      = historical/reference builder during migration;
                    future production-signing-host candidate only
Pixel 7 / panther = primary R8 runtime target
Titan 2           = second R8 portability/runtime target
```

Remove OptiPlex from the planned trust architecture.

Production AVB keys, OTA keys, production application keys, `sign_target_files_apks`, key backup/recovery/rotation and release handoff are **deferred** until Panther and Titan 2 development builds and runtime behavior are satisfactory.

Development/test signing remains necessary for functional engineering images, but it must not be confused with production signing architecture.

Do not call the ThinkPad `sable-signer-01` until the signing role has actually been designed, secured and commissioned.

## 9. Build-output / failure policy

- isolate OUT_DIR per target and materially different build variant;
- do not clean/clobber/delete as a reflexive retry;
- preserve valuable output after late failures;
- classify Kotlin/Rust, Soong graph, product selection, packaging/image, storage and runtime failures separately;
- retry the narrowest valid target after fixing the root cause;
- do not assume broad targets such as target-files are cheap without dependency/graph evidence;
- detect the actual image filesystem before choosing inspection tooling (for example ext4 vs EROFS).

## 10. Release/signing model

R8 is a development-integration milestone. Production signing is not a prerequisite for R8 application/product architecture closure.

A later release-signing workstream must separately define:

```text
production APK key policy
AVB key hierarchy
OTA signing
key custody/backup/recovery/rotation
approved artifact handoff
signing-host hardening/offline policy
signed-output provenance
```

## 11. R9 direction

R9 is the next coherent productivity/application tranche, not the first Calculator milestone. Candidate work remains Notes, Voice Notes, Flashcards, selected sensor games, Calendar after provider/data/permission design, future Sable Study/PDF workflow and selected Sable Start improvements.

## 12. Anti-drift rule

Before implementation/integration, answer:

```text
What requirement is being satisfied?
Which repository/layer owns it?
A1, A2, B1, B2/B3, or later release work?
What remains TBD?
What permission/network/data authority changes?
What exact source/artifact identity is tested?
What evidence closes this layer?
What later claim remains unproven?
```

Compilation is necessary evidence. It is never sufficient evidence for product inclusion, image membership, runtime correctness or release closure.
