# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible
mobile OS built around one common product core, narrow privilege boundaries and
separate touch-first / keyboard-first interaction profiles.

> Applications provide capabilities; Sable organizes people, attention and actions.

## Current state — 2026-09-25

```text
Pixel 7 / Panther      R9 HUB V1 PHYSICAL ACCEPTANCE PASS / FROZEN TOUCH-FIRST REFERENCE
Titan 2                ACTIVE KEYBOARD-FIRST N0 PORTABILITY TARGET
Titan 2 Elite          NEXT INDEPENDENT KEYBOARD-FIRST N0 TARGET
Q27                    RESEARCH / FUTURE PRODUCT CANDIDATE
Local direct CI        ACTIVE
Production signing     DEFERRED
```

Panther is no longer the active feature-design target. It remains the accepted
touch-first reference for regression, security, product and architecture
comparison.

The current private integration image authority is:

```text
R9_PANTHER_ACCEPTED_SOURCE=edf62e5bb08372a1395841d6cc5d78d3148a7695
R9_PANTHER_TARGET_FILES_SHA256=a0b359613c4f30e9a834fba212e0b044a97d63ed0537c59471c31b99b627d285
R9_PANTHER_HUB_V1_CLOSURE=MERGED_PR_110
R10_KEYBOARD_FIRST_DESIGN_V1=MERGED_PR_108
```

Public demo:

- [Panther R9 Hub V1 overall UI/UX demo](https://github.com/sableos-project/.github/releases/tag/panther-r9-hub-v1-demo-20260925)

This video is a product/UI demonstration for the accepted Panther R9 Hub V1
reference. It is not a new image qualification artifact and does not supersede
the accepted source or target-files identity above.

Later documentation-only and keyboard-first planning commits do not become new
Panther image qualification sources.

The active product question is now: **how does the same Sable product become a
first-class physical-keyboard OS without becoming a separate ROM/application
fork?**

## Product architecture

```text
common Sable applications + semantic contracts
        |
        +-- touch-first profile      -> Panther reference
        |
        +-- keyboard-first profile   -> Titan 2 / Titan 2 Elite / future Q27
        |
        v
bounded device adapters
        |
        v
Android framework + vendor HAL/BSP + hardware
```

Keyboard-first work adds deterministic focus, type-to-search, shortcuts,
square-display behavior, an image-integrated Sable Keyboard/input stack and a
capability-driven Sable Camera while preserving common app/data semantics.

## Sable Hub V1

The accepted communications product surface is:

```text
Priority | Messages | Email | People
```

Sable Hub is an aggregator and interaction surface. Sable Mail owns mail
accounts, protocols, message storage and credentials. Hub consumes bounded local
summaries and/or notification-derived state and must not scrape private provider
databases or manufacture delivery/read semantics.

## Device assurance

Non-Pixel development uses explicit levels:

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

A successful GSI boot is not a production-support claim. A Panther PASS never
implies Titan 2 or Titan 2 Elite runtime, camera, keyboard, telephony or
performance PASS.

## Repositories

- **platform_manifest** — exact OS source composition and provenance.
- **platform_sable** — common semantic/design/portability contracts.
- **vendor_sable** — common product composition and imported app integration.
- **device_sable_panther** — frozen Pixel 7 reference adapter/evidence.
- **build** — trusted CI/build/artifact/deployment contracts.
- **packages_apps_SableStart** — historical/common Sable Start presentation
  source; current product HOME ownership is documented there.
- **.github** — organization status, roadmap, trust and publication policy.

Reusable Sable source is intended to converge here over time. The private
integration repository is retained temporarily for release composition,
pre-publication work and evidence-bound integration; it is not intended to
remain the permanent home of all reusable product code.

Start with:

- [Current release status](../docs/CURRENT_RELEASE_STATUS.md)
- [Documentation status](../docs/DOCUMENTATION_STATUS.md)
- [Source ownership and publication](../docs/SOURCE_OWNERSHIP_AND_PUBLICATION.md)
- [Development release plan](../docs/DEVELOPMENT_RELEASE_PLAN.md)
- [CI trust architecture](../docs/CI_TRUST_ARCHITECTURE.md)
- [Security & quality engineering](../docs/SECURITY_QUALITY_ENGINEERING.md)

## Multi-device foundation — current

K1/K2 is merged. Artifact registry schema v2 supports multiple artifact kinds;
deployment safety/evidence is common while partition/transport semantics are
device-adapter-owned. Panther is the qualified target-files/A-B adapter. Titan 2,
Titan 2 Elite and Q27 remain fail-closed for release artifact registration and
flashing until independently qualified.

## Open issue policy

Remaining open issues are intentionally left open until the Titan 2 SableOS
install path proves or supersedes them. This includes Titan qualification,
keyboard-first input/focus work, Sable Tools, Camera/Keyboard integration and
open Panther/Titan-shared polish follow-ups.

## Documentation authority

The 2026-09-25 organization-wide sync updates the public docs to the final
Panther R9 Hub V1 closure and merged keyboard-first design state. Historical
R8/R9 documents remain evidence records, not current execution authority.
