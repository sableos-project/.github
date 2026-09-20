# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible mobile OS built around a common Sable product core, narrow privilege boundaries, strong evidence/provenance rules and distinct touch-first / keyboard-first interaction profiles.

[![Local CI](https://img.shields.io/static/v1?label=CI&message=Local%20Direct&color=2ea44f&style=flat-square)](../docs/CI_TRUST_ARCHITECTURE.md)
[![R9 Launcher](https://img.shields.io/static/v1?label=R9%20Launcher&message=Visual%20PASS&color=2ea44f&style=flat-square)](../docs/CURRENT_RELEASE_STATUS.md)
[![Product Composition](https://img.shields.io/static/v1?label=Product%20Composition&message=PASS&color=2ea44f&style=flat-square)](../docs/CURRENT_RELEASE_STATUS.md)
[![Fresh Panther](https://img.shields.io/static/v1?label=Fresh%20Panther&message=IN%20PROGRESS&color=f0ad4e&style=flat-square)](../docs/CURRENT_RELEASE_STATUS.md)

[![CodeQL Security Scan](https://github.com/sableos-project/packages_apps_SableStart/actions/workflows/codeql.yml/badge.svg?branch=main)](https://github.com/sableos-project/packages_apps_SableStart/actions/workflows/codeql.yml)
[![OWASP MobSF Scan](https://github.com/sableos-project/packages_apps_SableStart/actions/workflows/mobsfscan.yml/badge.svg?branch=main)](https://github.com/sableos-project/packages_apps_SableStart/actions/workflows/mobsfscan.yml)
[![License](https://img.shields.io/static/v1?label=License&message=per-component&color=007ec6&style=flat-square)](../docs/LICENSING.md)
[![Platform](https://img.shields.io/static/v1?label=Platform&message=Android%2017%20%2F%20SDK%2037&color=3DDC84&style=flat-square&logo=android&logoColor=white)](../docs/CURRENT_RELEASE_STATUS.md)
[![Optimized for](https://img.shields.io/static/v1?label=Optimized%20for&message=Pixel%207%20%2F%20Panther&color=111111&style=flat-square)](https://github.com/sableos-project/device_sable_panther)
[![Interaction](https://img.shields.io/static/v1?label=Interaction&message=Touch%20%2B%20Keyboard-First&color=6f42c1&style=flat-square)](../docs/CURRENT_RELEASE_STATUS.md)

> Applications provide capabilities; Sable organizes people, attention and actions.

## Badge scope

The live **CodeQL** and **OWASP MobSF** badges above report the public
`packages_apps_SableStart` source-security workflows. They are supplemental
source-analysis signals, not the authoritative Panther build/release gate.
SableOS product qualification remains local-direct and source-bound as described
below.

The license badge intentionally says **per-component**. SableOS currently has no
single organization-wide license grant; upstream/reused components retain their
applicable licenses and notices. See [Licensing](../docs/LICENSING.md).

## Current release

**R9** is the active development milestone.

R8 established the first-party application/design/product-composition foundation. R9 moves Sable Start onto the mature Launcher3/Quickstep HOME/Recents foundation, closes the production visual identity, proves a genuinely fresh Panther build, and then validates the exact image on a physical Pixel 7.

Current status:

```text
LOCAL_DIRECT_CI=ACTIVE
GITHUB_HOSTED_BUILD_CI=RETIRED
SELF_HOSTED_GITHUB_ACTIONS_RUNNER=DISABLED

R9_LAUNCHER3_FOUNDATION=PASS
R9_SABLESTART_VISUAL_REVIEW_SET=PASS
R9_SABLESTART_PRODUCT_IDENTITY=APPROVED
R8_FIRST_PARTY_PRODUCT_COMPOSITION=PASS

R9_FRESH_PANTHER_FULL_BUILD=IN_PROGRESS
R9_PIXEL7_PHYSICAL_ACCEPTANCE=PENDING
TITAN2_KEYBOARD_FIRST_PORTABILITY=QUEUED_AFTER_PANTHER
PRODUCTION_SIGNING=DEFERRED
```

See **[Current release status](../docs/CURRENT_RELEASE_STATUS.md)** for the exact claim boundaries.

## CI is a product feature

SableOS treats CI/build qualification as part of the product architecture rather than a green checkmark at the end.

The current model is intentionally local and evidence-bound:

```text
GitHub
  source + review + issues + documentation

controlled build machine
  one pinned host toolchain
  local CI lanes
  trusted app builds
  Soong/product gates
  source-bound fresh Panther builds
  hash/evidence retention

physical devices
  separately authorized runtime acceptance
```

GitHub-hosted Actions are no longer the authoritative build path, and a GitHub self-hosted runner is not active. Future remote execution must satisfy the existing encryption/isolation trust requirements before activation.

The canonical release command is release-neutral:

```bash
bash build/panther/run-release.sh R9
```

Only the release identifier changes between milestones.

## Current product architecture

### Sable Start

Launcher3/Quickstep owns Android HOME, Overview/Recents, task/gesture integration and launcher lifecycle. Sable owns the product presentation rendered in the NORMAL launcher state:

- Start;
- All Apps + Sable Rail;
- Search / Command;
- Sable Peek;
- Local Context;
- Follow System / Light / Dark + bounded accent appearance.

The standalone SableStart HOME APK is retired from the product graph.

### First-party application set

The current Panther product composition includes:

- Sable Calculator — standard, scientific and conversion;
- Sable Sudoku;
- Sable Minesweeper;
- Sable 2048;
- Sable Media;
- Sable Reader;
- Sable Hub / Messages;
- Sable Mail.

Application compilation, product selection, target-files membership and physical runtime remain separate claims.

## Hardware direction

**Pixel 7 / panther** is the primary Android 17 full-stack reference.

**Titan 2** is the next portability lab and is explicitly keyboard-first:

```text
navigation.primary=keyboard
navigation.secondary=touch
```

Its first purpose is to validate physical-QWERTY focus/navigation, printable-key type-to-search, pointer/touch coexistence and square/compact layout behavior using the same common Sable product contracts. Titan 2 mutation remains gated behind Panther R9 physical acceptance.

## Repositories

- **platform_manifest** — exact OS source composition and release-input provenance.
- **packages_apps_SableStart** — Sable Start presentation/source history and launcher requirements.
- **platform_sable** — common design, semantic, application and portability contracts.
- **vendor_sable** — common Sable product composition and imported app integration.
- **device_sable_panther** — Pixel 7-specific adaptation and runtime qualification.
- **build** — trusted build, local CI, reconstruction, product/freshness/evidence tooling.
- **.github** — organization roadmap, trust, assurance and documentation authority.

## Engineering principles

- Preserve proven Android/vendor capability unless replacement has a concrete product/security reason.
- Keep privileged execution behind narrow typed services and Android enforcement boundaries.
- Use Rust where risk/correctness justify it, not as branding.
- Never collapse source PASS, build PASS, product membership and runtime acceptance into one claim.
- A warmed incremental output is not fresh-build proof.
- Panther success is not Titan 2 portability proof.
- Production signing is a later release-security workstream, not a development shortcut.

Start with:

- [Current release status](../docs/CURRENT_RELEASE_STATUS.md)
- [Development release plan](../docs/DEVELOPMENT_RELEASE_PLAN.md)
- [CI trust architecture](../docs/CI_TRUST_ARCHITECTURE.md)
- [Security & quality engineering](../docs/SECURITY_QUALITY_ENGINEERING.md)
- [Documentation status](../docs/DOCUMENTATION_STATUS.md)
