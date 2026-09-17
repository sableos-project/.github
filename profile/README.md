# SableOS Project

SableOS is an experimental privacy- and security-focused Android-compatible operating system built around portable Sable-owned applications and product contracts while retaining Android's mature security, hardware, telephony, media and application-sandbox boundaries.

SableOS is developed with an evidence-first engineering model: least privilege, narrow trust boundaries, measurable performance, reproducible artifact provenance and layered automated/runtime testing are product requirements rather than release-time add-ons.

## Current state

The active development train is:

```text
R5/R6  source migration + real Sable Start foundation       historical base
R7     Panther product wiring + daily-driver evidence       evidence baseline
R8-A1  disposable GitHub app qualification                  established
R8-A2  trusted standalone app build on ai-g732              established
R8-B1  pre-image Android/Soong integration proof            established
R8-B2  Panther development image + qualification            reference PASS
R8-INFRA private-source/CI/security/reproducibility hardening ACTIVE
R8-UX  Sable application/shell evolution                    next implementation tranche
R8-B3  Titan 2 development image + portability qualification pending
LATER  production signing/release workstream
```

`R*` names are internal development/validation milestones, not public product versions.

Start with [`docs/DEVELOPMENT_RELEASE_PLAN.md`](../docs/DEVELOPMENT_RELEASE_PLAN.md), [`docs/REQUIREMENTS_INDEX.md`](../docs/REQUIREMENTS_INDEX.md) and [`docs/SECURITY_QUALITY_ENGINEERING.md`](../docs/SECURITY_QUALITY_ENGINEERING.md).

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

## Security, quality and performance engineering

SableOS does not equate a successful build, a scanner result, a percentage-coverage number or use of Rust with security. Assurance is layered and evidence-based.

The engineering contract includes:

- OWASP MASVS/MASTG-aligned mobile-security requirements and evidence mapping;
- least-privilege Android permissions, AppOps, exported-component and data-flow review;
- Rust-by-risk architecture with narrow, reviewed JNI/FFI boundaries;
- Rust `rustfmt`, Clippy, unit/property tests, RustSec advisory checks and targeted fuzzing;
- Android/Kotlin compilation, Android Lint, CodeQL, MobSF and additional code-quality/static checks;
- dependency provenance, immutable CI-action pinning, lock/verification policy and artifact hashing;
- Kover/cargo-llvm-cov coverage reporting with a baseline/ratchet policy rather than arbitrary vanity thresholds;
- Compose/instrumentation/UIAutomator tests plus physical-device acceptance;
- 16 KiB native-library compatibility checks;
- measured startup, frame/jank, memory, CPU/I/O and power behavior where relevant;
- fresh-build reproducibility and exact source/toolchain/artifact provenance across trust boundaries;
- strict separation between disposable PR CI, the trusted development builder, physical devices and any future production-signing environment.

Some controls are already implemented across SableOS repositories; others are explicitly listed as required infrastructure consolidation before the next large source expansion. Documentation distinguishes implemented controls from planned controls instead of presenting a roadmap item as a completed security guarantee.

See [`docs/SECURITY_QUALITY_ENGINEERING.md`](../docs/SECURITY_QUALITY_ENGINEERING.md) and [`docs/CI_TRUST_ARCHITECTURE.md`](../docs/CI_TRUST_ARCHITECTURE.md).

## Current R8 workstreams

The R8 application/shell plan is evolving from the validated Panther functional baseline. Current direction includes:

- **Sable Start / shell:** Metro-influenced, Sable-owned Start/Home, app list, search and related system-surface integration while retaining mature Android enforcement boundaries;
- **Calculator:** Standard + Scientific + offline conversion in one Sable Calculator product;
- **Games:** separate Sudoku, Mines and 2048 applications with deterministic Rust cores where useful;
- **Reader:** one Sable Reader product composing accepted publication and text/accessibility capabilities;
- **Media:** local Media + Internet Radio with Android-owned Media3/platform integration and Sable-owned deterministic domain logic where appropriate.

Substantial UX/source changes resume only after the private-source/CI/reproducibility infrastructure is canonicalized.

## Native portability

R8 native libraries must be verified compatible with 16 KiB page-size systems. The requirement is measured ELF/APK compatibility using the pinned toolchain, not one hard-coded linker flag for all environments.

Where compatible, Panther and Titan 2 should consume the same frozen common R8 app artifacts and the same common `vendor_sable` product composition. Device repositories own only real target-specific adaptation.

## Build and signing hosts

```text
GitHub hosted     = disposable/untrusted A1 CI
ai-g732           = trusted-development builder after infrastructure hardening
thinkpad-p50      = historical/reference builder during migration;
                    future production-signing-host candidate only
Pixel 7 / panther = primary R8 runtime target
Titan 2           = second R8 portability/runtime target
```

OptiPlex is no longer part of the planned signing architecture.

Production AVB/OTA/application signing is deliberately deferred until repeatable development builds and runtime behavior are satisfactory on both Panther and Titan 2. The ThinkPad must not be called `sable-signer-01` until that later role is actually designed, secured and commissioned.

## Repository architecture

- **`.github`** — organization roadmap, trust, security/quality engineering and product policy.
- **`platform_sable`** — shared Sable semantic/design/application architecture.
- **`platform_manifest`** — exact OS source composition and accepted external-artifact provenance.
- **`packages_apps_SableStart`** — Sable Start launcher/shell.
- **`vendor_sable`** — common product integration and selection of qualified applications.
- **`device_sable_*`** — bounded target adapters and runtime qualification.
- **`build`** — trusted build, reconstruction, pre-image/product-wiring and evidence tooling.

## Product principles

- Daily-driver reliability and Android security boundaries come before replacement branding.
- Privacy and least privilege are default requirements, not optional modes.
- Reuse proven code before rewriting it.
- Rust is used where it materially improves correctness/risk, not as a branding target.
- Android/Kotlin owns lifecycle, permissions, accessibility and framework integration where that boundary is safer.
- Security, code quality, coverage, fuzzing and performance are layered engineering controls, not one-number release claims.
- App compilation is not product-image proof.
- Product selection is not image membership; image membership is not runtime correctness.
- Panther success is not automatically Titan 2 portability proof.
- Production signing is a later release-security workstream, not an R8 development dependency.
- Requirements define behavior; evidence records whether it was achieved.

For current-vs-historical document classification see [`docs/DOCUMENTATION_STATUS.md`](../docs/DOCUMENTATION_STATUS.md).
