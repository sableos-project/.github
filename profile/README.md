# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible operating system built around portable Sable-owned applications and product contracts, while retaining Android's mature security, hardware, telephony, media, and application-sandbox boundaries.

## Current state

The project has moved beyond the original "theme-only R8, Calculator in R9" plan. The current development train is:

```text
R5/R6  source migration + real Sable Start foundation        historical/established base
R7     Panther product wiring + daily-driver qualification   current evidence baseline
R8     design + native application qualification             ACTIVE
        -> exact application/artifact freeze
        -> product-wiring proof
        -> one Panther integration image
        -> one Device1 integration campaign
R9+    next coherent productivity/replacement tranche        future
```

`R*` names are internal development/validation milestones, not public SableOS product versions. Exact build/release identity is revision-, manifest-, artifact-, device-, and signing-based.

The organization-wide roadmap is [`docs/DEVELOPMENT_RELEASE_PLAN.md`](../docs/DEVELOPMENT_RELEASE_PLAN.md). Start implementation work with [`docs/REQUIREMENTS_INDEX.md`](../docs/REQUIREMENTS_INDEX.md).

## R8 execution model

R8 deliberately separates application development from the Android image build.

```text
PROCESS A — standalone application qualification

Rust/Kotlin/application source
    -> deterministic tests
    -> static/security checks
    -> standalone APK/native builds
    -> manifest/package/permission inspection
    -> exact APK SHA-256 + source/workflow identity
    -> R8 application integration freeze

PROCESS B — SableOS product integration

frozen qualified inputs
    -> prove Android 17 / GrapheneOS integration mechanism
    -> prove product selection/install/target-files/image wiring
    -> one normal Panther image build
    -> one bounded Panther runtime/device campaign
```

The Panther/AOSP tree is therefore an integration and OS-build environment, not the everyday compiler for independently developed Rust/Kotlin applications.

The detailed R8 reuse/integration architecture is maintained in `platform_sable` and the organization application-reuse plan.

## Current R8 workstreams

- **R8-A — shared Sable design foundation:** Follow system / Light / Dark, bounded accent selection, reset, semantic design roles, accessibility and anti-drift rules.
- **R8-B — Calculator + Convert:** interaction-neutral exact arithmetic primitives plus portable conversion logic; unresolved calculator interaction semantics remain requirements decisions rather than being invented in code.
- **R8-C — Sable Games:** Sudoku, Minesweeper and 2048 with deterministic Rust rule/state cores and Android/Compose presentation.
- **R8-D — Sable Reader publication path:** reuse Vaachak Mobile / Readium for EPUB/library/reader behavior rather than writing another Android EPUB engine.
- **R8-D2 — Reader text/accessibility path:** reuse qualified Vaachak Text Reader capabilities for TXT, Android share/process-text, TTS and OCR, while treating translation/model-download/network behavior as a separate privacy policy gate.
- **R8-E — Sable Media:** local Music + Internet Radio; portable domain/parsing logic may be Rust, while Android owns Media3, MediaSession, storage, lifecycle, routing and networking.

One shipping Sable Reader product should compose the proven R8-D and R8-D2 capabilities rather than exposing two competing Sable Reader applications.

## Build and trust transition

The intended trusted Android image builder for the next R8 integration build is **`ai-g732`**, after the build/source/output environment is migrated onto the expanded storage and its identity is sealed.

During that transition:

```text
GitHub hosted     = disposable application/static/security qualification
ai-g732           = intended sable-builder-01 for R8 Android/product builds
thinkpad-p50      = legacy/reference build host and historical evidence source
Pixel 7 / panther = sable-device-01
OptiPlex          = sable-signer-01
```

A full R8 image build is not authorized merely because application CI is green. The integration input set must be frozen and the exact Android prebuilt/product wiring must first be proven.

## Repository architecture

- **`.github`** — organization-wide current-state, roadmap, trust, contribution and product-policy documentation.
- **`platform_sable`** — shared Sable semantic/design/application architecture and release/support contracts.
- **`platform_manifest`** — exact OS source composition plus release/build input provenance.
- **`packages_apps_SableStart`** — Sable Start launcher/shell source and launcher-specific requirements/evidence.
- **`vendor_sable`** — common product composition and qualified application integration; not application source ownership.
- **`device_sable_panther`** — bounded Panther product adapter/runtime qualification boundary.
- **`build`** — host build orchestration, authorization, reconstruction and evidence tooling.

Substantial Sable applications may receive dedicated organization repositories once their source/ownership boundary is stable. Do not create repository structure merely to get ahead of unresolved architecture.

## Current reference product target

Google Pixel 7 (`panther`) on the Android 17 / GrapheneOS-derived reference line remains the PRIMARY development/device qualification target. Common Sable behavior must remain portable rather than being forked into Panther-specific code.

## Product principles

- Daily-driver reliability and Android security boundaries come before branding replacement.
- Reuse proven code before rewriting it.
- Rust is used where it materially improves correctness/risk, not as a branding target.
- Android/Kotlin owns Android lifecycle, permissions, accessibility and framework integration.
- Application compilation success is not product-image proof.
- Product selection is not image membership; image membership is not runtime correctness.
- Complex inherited Phone/Messaging/Browser/Camera components remain until a separately justified replacement is qualified.
- Requirements define behavior; evidence records whether it was achieved.

For the current document hierarchy and historical-vs-current classification, see [`docs/DOCUMENTATION_STATUS.md`](../docs/DOCUMENTATION_STATUS.md).