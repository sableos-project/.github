# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible
mobile OS built around one common product core, narrow privilege boundaries and
separate touch-first / keyboard-first interaction profiles.

> Applications provide capabilities; Sable organizes people, attention and actions.

## Current state — 2026-09-24

```text
Pixel 7 / Panther      R9 PHYSICAL ACCEPTANCE PASS / FROZEN REFERENCE
Titan 2                ACTIVE KEYBOARD-FIRST PORTABILITY TARGET
Titan 2 Elite          NEXT INDEPENDENT KEYBOARD-FIRST TARGET
Q27                    RESEARCH / FUTURE PRODUCT CANDIDATE
Local direct CI        ACTIVE
Production signing     DEFERRED
```

Panther is no longer the active feature-design target. It remains the accepted
touch-first reference for regression, security and architecture comparison.

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

The accepted R9 launcher split is standalone `org.sableos.launcher` for HOME,
with Launcher3/Quickstep retained privately for Recents/task/gesture substrate.

Keyboard-first work adds deterministic focus, type-to-search, shortcuts,
square-display behavior, an image-integrated Sable Keyboard/input stack and a
capability-driven Sable Camera while preserving common app/data semantics.

## Device assurance

Non-Pixel development uses explicit levels:

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

A successful GSI boot is not a production-support claim.

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
- [Source ownership and publication](../docs/SOURCE_OWNERSHIP_AND_PUBLICATION.md)
- [Development release plan](../docs/DEVELOPMENT_RELEASE_PLAN.md)
- [CI trust architecture](../docs/CI_TRUST_ARCHITECTURE.md)
- [Security & quality engineering](../docs/SECURITY_QUALITY_ENGINEERING.md)
