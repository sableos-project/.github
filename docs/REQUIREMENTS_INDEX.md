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
- required/non-required scope;
- repository ownership;
- anti-drift/change-control rules;
- evidence/closure principles.

### Default application/replacement policy

Repository/path:

```text
sableos-project/.github
docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md
```

Read before choosing or replacing Phone, Messaging, Contacts, Browser, Camera, Files, Clock, Calculator, Weather, Maps, or other product applications.

It deliberately does not impose a blanket "AOSP" or "Graphene" application rule. Decisions are component-level.

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
- R5 open build/reconstruction claim boundary.

### Manifest reconstruction/composition requirements

```text
sableos-project/platform_manifest
docs/DEVELOPMENT_MILESTONE_COMPOSITION.md
```

Read for canonical project paths, revision pinning, local-manifest restrictions, and clean reconstruction expectations.

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

## R7 — Panther daily-driver qualification

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

## R8 — shared Sable design/theme/customization

Owning requirements:

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
- common consumption by Sable applications;
- no broad theme marketplace or launcher-customization explosion in first R8.

Read `platform_sable/docs/ARCHITECTURE.md` and `PORTABILITY_RULES.md` before introducing any new common service.

## R9 — first native Sable utilities

Owning common application model:

```text
sableos-project/platform_sable
docs/R9_SABLE_UTILITY_APP_MODEL.md
```

First planned app: **Sable Calculator**.

Locked direction includes:

- low privilege;
- no network/sensitive permission for basic Calculator;
- arithmetic semantics documented before implementation;
- numeric/rounding policy documented before implementation;
- deterministic logic tests;
- accessibility baseline;
- R8 design/theme consumption;
- dedicated canonical app repository when created;
- exact build/package/manifest integration evidence.

Notes/Clock/Files are later candidates only. Candidate status is not authorization to implement them.

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
- one device passing a feature does not automatically qualify another device or carrier.

## Implementation anti-drift checklist

Before writing code, answer all of these from GitHub:

```text
What milestone am I implementing?
What exact requirement is this change satisfying?
Which repository owns the behavior?
What is explicitly out of scope?
What Android/Sable/device boundary owns the implementation?
Does this add a permission/role/privilege/dependency?
Does this change product composition/default apps?
What exact tests/evidence close the requirement?
What source/build identity will the evidence bind to?
```

If an answer cannot be found, update the requirements before implementation.

## Documentation update rule

When a product decision changes, update the owning document and this index if the path/major milestone direction changes.

When a gate closes, update status/evidence documentation but do not rewrite the original requirement to make the observed implementation appear correct after the fact.

Requirements define the target; evidence records whether the target was met.