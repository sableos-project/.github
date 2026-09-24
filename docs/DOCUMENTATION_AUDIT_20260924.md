# SableOS organization documentation audit — 2026-09-24

Status: **completed deep-dive reconciliation**

## Scope

The audit reviewed every Markdown/text documentation file present on the default
`main` branches of all seven public `sableos-project` repositories before
this audit record was added.

```text
.github                         19
platform_manifest                6
packages_apps_SableStart        12
platform_sable                  13
vendor_sable                     3
device_sable_panther             4
build                            13
----------------------------------
TOTAL                           70
```

This audit file is the 71st documentation record created by the reconciliation.

## Current state used for reconciliation

```text
Panther / Pixel 7
    R9 physical acceptance PASS
    REFERENCE_FROZEN
    accepted image source:
      6f1d6d2f0f2525067874238c4b797ad58f2bcbc6
    accepted target-files SHA-256:
      08ef429c7f9eef17de7ddad4ce9baa81911950e8d34b6751b6e4588f66821502

K1/K2 multi-device foundation
    full CI source:
      dec78f0f6a86c0236cd4783b14efb7ee86e45215
    private integration merge:
      f11502202e8913bdb2e7317d824d26577a98e7a1
    status:
      MERGED / local full CI PASS

Titan 2
    PORTABILITY / N0 active research
    release artifact registration + flash still blocked

Titan 2 Elite
    independent PORTABILITY candidate / N0 pending
    release artifact registration + flash still blocked

Q27
    RESEARCH / future candidate
    release artifact registration + flash blocked
```

## Current product architecture used for reconciliation

```text
org.sableos.launcher / SableLauncher
    HOME / Start / All Apps / Search / Peek / app context

Launcher3QuickStep
    Recents / Overview / task / gesture substrate
    not HOME eligible

org.sableos.start / SableStart
    retired standalone runtime product
    historical/presentation reference
```

Reader/Text Reader are separate accepted products.

Settings is the global appearance authority.

Keyboard-first is a common interaction profile, not a device-specific
application fork.

## K1/K2 architecture used for reconciliation

K1 artifact registry v2 supports explicit artifact classes:

```text
target-files
full-device-images
gsi-system-image
system-product-bundle
boot-recovery-bundle
```

Physical device serial is not artifact identity.

K2 separates:

```text
common safety/evidence orchestration
    vs
device-owned transport / partition / AVB / restore semantics
```

Panther is the qualified target-files/A-B adapter. Titan 2, Titan 2 Elite and
Q27 remain fail-closed.

## Audit treatment rule

Current/normative files were rewritten when their body represented obsolete
state.

Historical evidence/planning files were **not rewritten to change the historical
record**. Instead they received explicit top-of-file classification indicating
that their original pending/PRIMARY/HOME/milestone wording is historical and
superseded for current-status purposes.

This preserves evidence while preventing accidental use as current authority.

## Major stale claims corrected

The reconciliation removed these claims from current authority documents:

- Panther is the current PRIMARY/active feature target;
- Panther R9 physical acceptance is pending;
- R9 fresh Panther build is still in progress;
- Launcher3/Quickstep owns user-facing HOME;
- SableStart remains the shipping HOME product;
- SableStart HOME adoption is still a pending project decision;
- Reader and Text Reader should ship as one Reader product;
- K1 artifact generalization is only future/planned work;
- K2 adapter-owned deployment split is only future/planned work;
- Panther target-files/A-B `flashall` semantics are the generic deployment
  model;
- Titan 2 simply follows the old R8 B3 sequence after Panther acceptance;
- R8 A1/A2/B1/B2/B3 is the active project execution plan;
- old R7 pending matrix cells describe current Panther project status.

## Repository results

### .github

Rebuilt current status, development plan, documentation classification, CI/trust
architecture, requirements index, default-app policy, Rust/Android architecture
and security/quality policy.

R8 review/program documents remain historical and are explicitly classified.

### platform_manifest

Rebuilt current composition policy, manifest hierarchy, release provenance and
source-composition model.

R5 reconstruction plan is explicitly historical.

### packages_apps_SableStart

Current README/architecture/status now describe the repository as
presentation/history reference rather than current HOME authority.

R3/R5/R6 migration/runtime/HOME-adoption material is explicitly historical.

### platform_sable

Rebuilt common architecture/release model and current README.

Aligned Camera, Keyboard, device support and portability with keyboard-first
architecture and K1/K2.

Replaced the obsolete one-Reader model with the accepted Reader/Text Reader
split.

Old R8/R9 milestone documents are explicitly historical.

### vendor_sable

Current default application composition now matches the accepted Panther
reference and current common app family.

Ownership boundary reflects multi-device artifact/deployment architecture.

### device_sable_panther

Panther is explicitly REFERENCE_FROZEN and bound to the exact accepted
source/artifact.

R7 matrix is historical; current validation model is regression/reference
oriented rather than pending-R9 oriented.

### build

Current docs describe implemented artifact registry schema v2, adapter capability
gates, exact serial binding and common-policy/device-transport deployment split.

R8 command/pre-image/review files are explicitly historical.

## Repository heads inspected after reconciliation edits and before this audit
record commit

```text
sableos-project/.github
  97e9d4ee51f4e217d959f23dabc86caf73fad849

sableos-project/platform_manifest
  7c98a4935976e00bb84d612aafd74d494979a40c

sableos-project/packages_apps_SableStart
  3354cf4caa1733b6f4fe93b878f11d97dd76018e

sableos-project/platform_sable
  491cce709f568709919a98ffbcac0737a48778da

sableos-project/vendor_sable
  e0d78df73fed6a8c1c2d39a5b0a5a8f2601ed905

sableos-project/device_sable_panther
  2a9df41804be2bcef6efc1728ba425dd066e3548

sableos-project/build
  9e8d2ca9baee8cf521be01639bb2e5c72597811f
```

These are source/documentation snapshots, not new Panther image qualification
identities.

## Current authority chain

```text
.github/docs/CURRENT_RELEASE_STATUS.md
  -> .github/docs/DEVELOPMENT_RELEASE_PLAN.md
  -> .github/docs/DOCUMENTATION_STATUS.md
  -> platform_sable current architecture/support/portability/release docs
  -> build current artifact/deployment/evidence docs
  -> vendor_sable common product composition
  -> device adapter current docs
  -> historical milestone/evidence files only for historical investigation
```

## Closure

```text
SABLEOS_PUBLIC_DOCS_REVIEWED=70
SABLEOS_PUBLIC_REPOSITORIES_REVIEWED=7
SABLEOS_CURRENT_STALE_PANTHER_PENDING=ABSENT
SABLEOS_CURRENT_STALE_PANTHER_PRIMARY=ABSENT
SABLEOS_CURRENT_STALE_LAUNCHER3_HOME=ABSENT
SABLEOS_CURRENT_STALE_SABLESTART_HOME=ABSENT
SABLEOS_CURRENT_STALE_ONE_READER_MODEL=ABSENT
SABLEOS_CURRENT_STALE_K1_K2_FUTURE_ONLY=ABSENT
SABLEOS_HISTORICAL_RECORDS_PRESERVED=YES
SABLEOS_DOCUMENTATION_RECONCILIATION=PASS
```


## Post-reconciliation verification

A second pass re-read every current/normative authority document after the
updates and checked specifically for obsolete current-state assertions.

Verified absent from current authority:

```text
R9 is active / Panther acceptance pending
fresh Panther build in progress
Panther is current PRIMARY
Launcher3/Quickstep owns HOME
SableStart remains shipping HOME
one-Reader product model
K1/K2 described as future-only
generic deployment described as Panther flashall
```

The phrase "No new PRIMARY is declared" in the current device-support document
was reviewed as an intentional negation, not a stale PRIMARY claim.

Historical documents still contain original R3-R8/R9 wording where required for
evidence fidelity, but each such file now carries an explicit historical or
superseded classification at the top.

```text
SABLEOS_CURRENT_AUTHORITY_SECOND_PASS=PASS
SABLEOS_CURRENT_AUTHORITY_STALE_CLAIM_COUNT=0
```
