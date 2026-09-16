# SableOS requirements index

Status: **entry point for future implementation sessions.**

Use this file before starting a new milestone. The intent is that a future engineer or ChatGPT session can recover current product direction from GitHub instead of reconstructing decisions from chat history.

## Required reading protocol

Before source mutation for a milestone:

1. read the organization-wide development plan;
2. read the owning repository's milestone requirements;
3. read the relevant architecture/ownership documents;
4. read current migration/validation status;
5. identify exact source/build/device baseline;
6. write down any requirement that is still ambiguous;
7. update documentation before implementing a new product semantic that is not already decided.

Do not use "reasonable default" as a substitute for a recorded requirement when the choice changes product behavior, privilege, data ownership, compatibility, or release composition.

## Organization-wide direction

### Product development train

Repository/path:

```text
sableos-project/.github
docs/DEVELOPMENT_RELEASE_PLAN.md
```

Defines:

- daily-driver-first objective;
- R5 -> R6 -> R7 -> R8 -> R9 -> R10+ order;
- product layering;
- milestone dependencies;
- consolidated R8 native-application foundation;
- fewer full Panther builds through coherent integration freezes;
- required/non-required scope;
- repository ownership;
- anti-drift/change-control rules;
- evidence/closure principles.

### Application reuse and integration plan

Repository/path:

```text
sableos-project/.github
docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md
```

Read before implementing or integrating Sable Calculator/Convert, Games, Reader, Media, Dictionary, or another application derived from an existing owned project.

Locked direction includes:

- reuse proven first-party code before rewriting;
- host/Gradle/Cargo verification before Panther product builds;
- one coherent integration build per application tranche rather than one full build per small feature;
- `rustmix-wave` as a reuse source for native games/converter/dictionary logic;
- `rustmix-x4-firmware` as a source of reader/domain concepts rather than the primary Android renderer;
- `vaachak-mobile` as the primary Android/Compose/Readium source for Sable Reader;
- ESP32 Assistant as a reuse source for Music/Internet-Radio domain/state/parsing logic, not its embedded playback backend;
- Android ownership of Android lifecycle/media/storage/sensor integration;
- **no Lua runtime in SableOS applications**;
- PDF remaining separate from Sable Reader in R8;
- exact standalone-app artifact/source provenance before product integration.

### Rust/Kotlin application architecture

Repository/path:

```text
sableos-project/.github
docs/RUST_APPLICATION_ARCHITECTURE.md
```

Read before adding Rust, JNI, Binder/native boundaries, unsafe code, Cargo dependencies, parser/fuzzing work, or replacing a Kotlin/platform component merely to increase Rust usage.

The governing principle remains `RUST_BY_RISK, NOT_RUST_BY_BRANDING`.

### Default application/replacement policy

Repository/path:

```text
sableos-project/.github
docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md
```

Read before choosing or replacing Phone, Messaging, Contacts, Browser, Camera, Files, Clock, Calculator, Weather, Maps, PDF/document components, or other product applications.

It deliberately does not impose a blanket "AOSP" or "Graphene" application rule. Decisions are component-level.

### CI/build/device/signing trust architecture

Repository/path:

```text
sableos-project/.github
docs/CI_TRUST_ARCHITECTURE.md
```

This is required reading before adding or modifying GitHub Actions, self-hosted runners, device automation, release signing, or build-cache sharing.

Formal infrastructure identity:

```text
thinkpad-p50      = sable-builder-01
optiPlex          = sable-signer-01
Pixel 7 / panther = sable-device-01
GitHub hosted     = untrusted/disposable CI
```

Locked trust direction:

- untrusted PR code executes only on disposable GitHub-hosted infrastructure;
- `sable-builder-01` executes only explicitly trusted source identities;
- the current developer/reference AOSP workspace is not a generic Actions scratch tree;
- future automated ThinkPad CI uses a dedicated account/workspace boundary;
- `sable-device-01` consumes exact hash-identified artifacts under separately authorized device tests;
- `sable-signer-01` is a signing appliance, not a general build/CI host;
- production signing material does not live on GitHub-hosted runners or the normal builder;
- third-party Actions are pinned to full commit SHA and run with least privilege.

The implementation-side CI execution model is maintained in `sableos-project/build/docs/CI_EXECUTION_MODEL.md`.

## R5 — migrated-source build/reconstruction closure

### Sable Start migration state

```text
sableos-project/packages_apps_SableStart
docs/MIGRATION_STATUS.md
```

Read for:

- sealed R3 source identity;
- R4 capture evidence;
- R4-R1 commit identity;
- R4-R2 push/PR status;
- R5-R2 direct migrated-checkout build PASS;
- remaining clean reconstruction/source-integration boundary.

### Manifest reconstruction/composition requirements

```text
sableos-project/platform_manifest
docs/DEVELOPMENT_MILESTONE_COMPOSITION.md
docs/R5_R3_RECONSTRUCTION_PLAN.md
```

The R5-R3 audit established that `platform_manifest` still lacks the operational `default.xml` / common / Panther / release manifest hierarchy. `R5_R3_RECONSTRUCTION_PLAN.md` defines the exact GrapheneOS `2026081300` substrate binding, SableStart path/revision mapping, clean reconstruction phases, artifact expectations, and closure statement.

Before manifest mutation, run the read-only historical-workspace audit gate:

```text
sableos-project/build
gates/r5_r3a_manifest_audit.sh
```

It records `.repo` manifest identity, resolved manifest, local-manifest inventory, `packages/apps/SableStart` path ownership/collision state, and evidence hashes without sync/build/source mutation.

### Build/evidence gates

```text
sableos-project/build
docs/MILESTONE_EVIDENCE_GATES.md
```

Read for R5 build-input identity, artifact proof, sandbox/host limitation classification, evidence sealing, and reconstruction requirements.

## R6 — real Sable Start launcher

Owning requirements:

```text
sableos-project/packages_apps_SableStart
docs/R6_ALL_APPS_AND_GREETING.md
```

Locked direction includes:

- All Apps = Android launcher-visible activities for accessible profiles, not all installed packages;
- real labels/icons;
- deterministic ordering;
- visible inventory count;
- live package/profile refresh;
- All Apps and Search share one live inventory;
- exact component/profile launch semantics;
- local phone-time greeting:
  - 05:00–11:59 Good morning
  - 12:00–16:59 Good afternoon
  - 17:00–21:59 Good evening
  - 22:00–04:59 Good night;
- Maps/Weather may remain installed as third-party inventory fixtures;
- no theme editor, folders/categories, cloud search, recommendation engine, or custom Phone/Messaging in R6.

Before coding R6, also read `packages_apps_SableStart/docs/ARCHITECTURE.md` and current `MIGRATION_STATUS.md`.

## R7 — Sable Start production surfaces + Panther daily-driver qualification

Launcher requirements:

```text
sableos-project/packages_apps_SableStart
docs/R7_PRODUCTION_SURFACES.md
```

Owning runtime matrix:

```text
sableos-project/device_sable_panther
docs/R7_DAILY_DRIVER_VALIDATION.md
```

Required baseline covers:

- incoming/outgoing calls and audio;
- contacts workflows;
- SMS;
- MMS;
- Wi-Fi;
- cellular data;
- Wi-Fi/cellular transition;
- browser/Internet;
- notifications;
- Settings;
- camera/photos;
- files;
- clock/alarm;
- calculator;
- Sable Start integration.

### Product/default-app integration policy

```text
sableos-project/vendor_sable
docs/DEFAULT_APPLICATION_COMPOSITION.md
```

Read before changing product package lists, default roles, privileged permission allowlists, overlays, or app replacements.

### Default-app decision framework

Also read:

```text
sableos-project/.github
docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md
```

R7 must record actual selected package/component/provenance/privilege/dependencies/maintenance status. `TBD` is preferable to an invented blanket upstream choice.

## R8 — shared Sable design + native application foundation

R8 is now a consolidated integration tranche rather than a design-only milestone followed immediately by a separate one-app product-build milestone.

### Shared design/customization requirements

```text
sableos-project/platform_sable
docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md
```

Locked first-stage direction includes:

- semantic color roles;
- typography roles;
- spacing/shape/icon/motion guidance;
- Follow system / Light / Dark;
- bounded accent selection;
- typed/persisted product-owned settings;
- accessibility requirements;
- common consumption by Sable Start and R8 Sable applications;
- no broad theme marketplace or launcher-customization explosion in first R8.

### Application reuse/build policy

```text
sableos-project/.github
docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md
```

R8 application workstreams are:

- **Sable Calculator + Convert** — deterministic arithmetic and conversion behavior, no network/sensitive permission for core functionality;
- **Sable Games** — initial Sudoku, Minesweeper, and 2048, reusing/refactoring native Rust game rules from `rustmix-wave`; one initial Games APK; no Lua runtime;
- **Sable Reader** — reuse the existing Vaachak Android/Compose/Readium reader for EPUB/TXT through shared source/product adaptation rather than creating a second Android EPUB renderer;
- **PDF** — remain on the inherited/proven secure PDF viewer in R8; future Sable Study is a separate decision;
- **Sable Media** — Music + Internet Radio, reusing portable domain/state/parsing from the ESP32 Assistant while Android owns playback, MediaSession, storage, routing, and networking;
- **Sable Dictionary** — desirable offline shared service/app workstream, but explicitly deferable if data/provenance/shared ownership would delay the integration freeze.

### R8 pre-build and integration rule

Before the R8 Panther build, applicable workstreams must pass host/application gates:

```text
Rust: rustfmt + clippy + cargo tests + targeted property/fuzz tests
Kotlin/Gradle: unit/state/static/Compose tests + standalone APK build
Android integration: narrow Soong only when platform/resource/JNI semantics require it
```

Then freeze exact source/artifact identities and perform **one normal Panther integration build** followed by one Device1 integration campaign.

Do not treat `target-files-package` as a presumed cheap packaging operation; broad product targets may approach a full build and must be budgeted accordingly.

Read `platform_sable/docs/ARCHITECTURE.md`, `PORTABILITY_RULES.md`, `docs/RUST_APPLICATION_ARCHITECTURE.md`, and the application reuse plan before introducing new common services or JNI boundaries.

## R9 — next coherent application/productivity tranche

R9 is no longer defined as "Calculator, then another full build." Calculator is part of R8.

R9 candidates include:

- Notes;
- Flashcards implemented without a Lua runtime;
- Voice Notes using Android audio APIs plus reusable domain logic;
- Tilt Maze / Sokoban with Android sensor adapters;
- Calendar after provider/permission/data ownership is explicit;
- selected Sable Start improvements;
- Sable Study if richer PDF annotation/highlighting/note workflows justify it.

The selected R9 set must be documented before integration freeze. Candidate status is not automatic implementation authorization.

R9 uses the same build discipline as R8:

```text
host/app qualification
    -> exact source/artifact freeze
    -> one coherent Panther integration build
    -> one device campaign
```

## R10+ — deliberate replacement/expansion

Use the organization-wide replacement threshold in:

```text
.github/docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md
```

and product cleanup/composition requirements in:

```text
vendor_sable/docs/DEFAULT_APPLICATION_COMPOSITION.md
platform_manifest/docs/DEVELOPMENT_MILESTONE_COMPOSITION.md
build/docs/MILESTONE_EVIDENCE_GATES.md
```

Phone/Messaging/Browser/Camera replacement is never implied merely because Sable-owned utilities exist.

## Release/build identity

Read:

```text
platform_sable/docs/RELEASE_MODEL.md
platform_sable/docs/DEVICE_SUPPORT_LEVELS.md
platform_manifest/docs/RELEASE_MANIFEST_POLICY.md
platform_manifest/docs/SOURCE_COMPOSITION_MODEL.md
build/docs/REPRODUCIBILITY.md
build/docs/AUTHORIZATION_MODEL.md
```

Remember:

- `R*` milestones are internal development/validation checkpoints;
- semantic SableOS product version is separate;
- branch names are not sufficient release provenance;
- exact manifests/component commits/artifact hashes identify builds;
- standalone application PASS is not product-image integration proof;
- one device passing a feature does not automatically qualify another device or carrier.

## Implementation anti-drift checklist

Before writing code, answer all of these from GitHub:

```text
What milestone am I implementing?
What exact requirement is this change satisfying?
Which repository owns the behavior?
Is there already first-party code we should reuse instead of rewrite?
What is explicitly out of scope?
What Android/Sable/device boundary owns the implementation?
Does this add a permission/role/privilege/dependency?
Does this change product composition/default apps?
Can the behavior be host/app verified before an Android product build?
What exact tests/evidence close the requirement?
What source/build/artifact identity will the evidence bind to?
```

If an answer cannot be found, update the requirements before implementation.

## Documentation update rule

When a product decision changes, update the owning document and this index if the path/major milestone direction changes.

When a gate closes, update status/evidence documentation but do not rewrite the original requirement to make the observed implementation appear correct after the fact.

Requirements define the target; evidence records whether the target was met.