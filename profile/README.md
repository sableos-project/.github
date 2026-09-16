# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible operating system built around portable Sable-owned applications and product contracts while retaining Android's mature security, hardware, telephony, media and application-sandbox boundaries.

## Current state

The active development train is now:

```text
R5/R6  source migration + real Sable Start foundation       historical base
R7     Panther product wiring + daily-driver evidence       evidence baseline
R8-A1  disposable GitHub app qualification                  ACTIVE
R8-A2  trusted standalone app build on ai-g732
        -> exact application freeze
R8-B1  pre-image Android/Soong integration proof
R8-B2  Panther development image + qualification
R8-B3  Titan 2 development image + portability qualification
LATER  production signing/release workstream
R9+    next coherent productivity/replacement tranche
```

`R*` names are internal development/validation milestones, not public product versions.

Start with [`docs/DEVELOPMENT_RELEASE_PLAN.md`](../docs/DEVELOPMENT_RELEASE_PLAN.md) and [`docs/REQUIREMENTS_INDEX.md`](../docs/REQUIREMENTS_INDEX.md).

## R8 execution model

```text
A1 — disposable qualification
GitHub-hosted Rust/Kotlin/Gradle/static/security CI
        |
        v
A2 — trusted standalone application build
ai-g732 with pinned toolchains
        |
        v
exact trusted application freeze
        |
        v
B1 — pre-image Soong/product integration proof
        |
        v
B2 — Panther development image + runtime acceptance
        |
        v
B3 — Titan 2 portability image + runtime acceptance
        |
        v
later production signing
```

GitHub artifacts are qualification evidence; they do not automatically enter the trusted product binary chain. The Panther/AOSP tree is an integration environment, not the everyday compiler for independently developed applications.

## Current R8 workstreams

- **R8-A — shared design:** Follow system / Light / Dark / bounded accent / reset, shared semantic roles and accessibility.
- **R8-B — Calculator + Convert.**
- **R8-C — Games:** Sudoku, Minesweeper and 2048.
- **R8-D — Reader publication path:** Vaachak Mobile / Readium.
- **R8-D2 — Reader text/accessibility:** TXT / share/process-text / TTS / OCR from Vaachak Text Reader capability.
- **R8-E — Media:** local Music + Internet Radio with Android-owned Media3/platform integration.

One Sable Reader product composes the accepted R8-D and R8-D2 capability paths.

## Native portability

R8 native libraries must be verified compatible with 16 KiB page-size systems. The requirement is measured ELF/APK compatibility using the pinned toolchain, not one hard-coded linker flag for all environments.

Where compatible, Panther and Titan 2 should consume the same frozen common R8 app artifacts and the same common `vendor_sable` product composition. Device repositories own only real target-specific adaptation.

## Build and signing hosts

```text
GitHub hosted     = disposable/untrusted A1 CI
ai-g732           = intended trusted A2/B1/B2/B3 development builder
thinkpad-p50      = historical/reference builder during migration;
                    future production-signing-host candidate only
Pixel 7 / panther = primary R8 runtime target
Titan 2           = second R8 portability/runtime target
```

OptiPlex is no longer part of the planned signing architecture.

Production AVB/OTA/application signing is deliberately deferred until development images and runtime behavior are satisfactory on both Panther and Titan 2. The ThinkPad must not be called `sable-signer-01` until that later role is actually designed, secured and commissioned.

## Repository architecture

- **`.github`** — organization roadmap, trust and product policy.
- **`platform_sable`** — shared Sable semantic/design/application architecture.
- **`platform_manifest`** — exact OS source composition and accepted external-artifact provenance.
- **`packages_apps_SableStart`** — Sable Start launcher/shell.
- **`vendor_sable`** — common product integration and selection of qualified applications.
- **`device_sable_*`** — bounded target adapters and runtime qualification.
- **`build`** — trusted build, reconstruction, pre-image/product-wiring and evidence tooling.

## Product principles

- Daily-driver reliability and Android security boundaries come before replacement branding.
- Reuse proven code before rewriting it.
- Rust is used where it materially improves correctness/risk, not as a branding target.
- Android/Kotlin owns lifecycle, permissions, accessibility and framework integration.
- App compilation is not product-image proof.
- Product selection is not image membership; image membership is not runtime correctness.
- Production signing is a later release-security workstream, not an R8 bring-up dependency.
- Requirements define behavior; evidence records whether it was achieved.

For current-vs-historical document classification see [`docs/DOCUMENTATION_STATUS.md`](../docs/DOCUMENTATION_STATUS.md).
