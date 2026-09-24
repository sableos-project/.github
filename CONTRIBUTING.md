# Contributing to SableOS

> **Current contribution context — 2026-09-24:** Panther is a frozen reference. K1/K2 is merged. New feature work should target common keyboard-first product architecture or bounded Titan research/adapters without enabling unqualified device mutation. Historical R8/R9 sequencing below is reference only where explicitly labeled.


SableOS uses evidence-driven development, requirements-first implementation, explicit repository ownership and a strict distinction between standalone application qualification and operating-system product integration.

## Read before code

Start with:

- `docs/REQUIREMENTS_INDEX.md`;
- `docs/DEVELOPMENT_RELEASE_PLAN.md`;
- the owning repository's architecture/requirements/status documents;
- `docs/DOCUMENTATION_STATUS.md` when an older milestone file may conflict with current direction.

If a product behavior, privilege, data-ownership rule or integration mechanism is still undecided, update the requirements/architecture before silently choosing it in code.

## Ownership

Place changes at the narrowest correct layer:

```text
application repository/workspace
    application source, tests, standalone build/dependency graph

platform_sable
    shared semantic/design/application architecture contracts

vendor_sable
    common product selection/integration of qualified inputs

device_sable_<target>
    genuine device-specific adaptation/qualification

platform_manifest
    exact OS source composition and qualified external input provenance

build
    build/reconstruction/validation tooling
```

Do not put common product semantics in a device tree. Do not put application source in `vendor_sable`. Do not patch generated substrate files merely because they are convenient.

## Process A — application qualification

Ordinary Rust/Kotlin application development should use the application's canonical Cargo/Gradle/source workflow first.

As applicable, changes should pass independently diagnosable checks for:

- formatting/static analysis;
- unit/property/fuzz tests;
- dependency/security/provenance checks;
- Android compile/tests;
- Android lint;
- upstream/reuse pin validation;
- APK manifest/package/permission/native-ABI inspection;
- artifact SHA-256 sealing.

A green standalone app build is not SableOS image inclusion evidence.

## Process B — product integration

Product-integration changes must bind exact qualified inputs and prove the relevant ladder:

```text
sealed source/artifact
 -> import/module declaration
 -> product selection
 -> PRODUCT_OUT
 -> installed-files / target-files
 -> image
 -> runtime
```

Do not launch a broad Android product build merely to discover an app compile error that belongs in Process A.

## Rust/Kotlin boundary

Follow `docs/RUST_APPLICATION_ARCHITECTURE.md`.

Use Rust where deterministic/high-risk domain logic materially benefits; use Kotlin/Android for framework lifecycle, permissions, accessibility, intents/providers, CameraX, Media3/MediaSession, Readium and similar Android integration.

JNI/FFI must stay narrow and tested. Do not add native boundaries for branding or language-percentage goals.

## Reuse before rewrite

Before implementing a new application/domain, check the current reuse plan. Existing Rustmix, Vaachak and ESP-derived work may provide proven behavior that should be extracted/adapted rather than rewritten.

First-party ownership does not waive third-party licensing, dependency, model/data or security obligations.

## Authorization

State explicitly which operations are authorized. Keep separate concepts for:

- source mutation;
- build-output mutation;
- network fetch;
- build execution;
- Git commit/push/merge;
- device contact;
- package install/uninstall;
- reboot;
- role/default-app changes;
- flashing/signing;
- root/remount/slot/wipe;
- clean/clobber/delete.

Permission for one category does not imply another.

## Evidence and claim boundaries

Every PR should say what was actually proved and what remains unproven.

Examples:

```text
cargo test PASS
    != Android JNI packaging PASS

APK assemble PASS
    != product integration PASS

product selection PASS
    != image membership PASS

image membership PASS
    != runtime correctness PASS
```

Bind important evidence to exact source/build/artifact identities.

## Historical documents

Do not rewrite historical PASS/FAIL results, hashes or acceptance boundaries merely because the current architecture changed. Add a supersession/status note or update the current documentation index instead.

## Full image builds

Full Android image builds are integration checkpoints, not ordinary application feedback loops. Panther is frozen; future Titan-family image work must retain source-bound qualification and adapter-owned artifact/deployment evidence.

`ai-g732` is the controlled local CI/build host. The ThinkPad P50 is historical/reference only. Production signing remains separately deferred.

## Pull requests

Use the organization PR template. A PR should identify:

- milestone/workstream;
- Process A / Process B / policy-only classification;
- owning layer;
- source/upstream/artifact identities;
- permissions/authority/dependency impact;
- validation completed;
- claim boundary;
- product/release impact;
- rollback/fallback when relevant.

Small, bounded, evidence-rich changes are preferred over broad changes whose ownership and validation cannot be stated precisely.