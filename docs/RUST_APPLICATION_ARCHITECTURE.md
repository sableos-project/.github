# SableOS Rust application architecture

Status: **normative product/security architecture for Rust and Kotlin/Compose application development.**

SableOS uses Rust where it materially reduces risk or improves correctness, and uses Kotlin/Compose where Android framework integration is safer, simpler, and more maintainable.

The governing rule is:

```text
RUST_BY_RISK, NOT_RUST_BY_BRANDING
```

Rust is not a product badge. Rewriting a component in Rust is justified only when the resulting ownership, privilege, maintenance, and test burden is safer or more maintainable than retaining the existing implementation.

## 1. Primary architecture

For Sable-owned Android applications, prefer this split when a meaningful Rust core exists:

```text
Kotlin / Jetpack Compose
    UI and accessibility semantics
    Activity/service lifecycle
    Android permissions and roles
    framework/provider APIs
    notifications/intents
    PackageManager / LauncherApps / Telecom / Camera / MediaStore integration
            |
            | narrow typed JNI/Binder boundary where justified
            v
Rust core
    deterministic domain logic
    parsers and codecs
    protocol/state machines
    validation
    security-sensitive transformations
    cryptographic verification
    selected persistence/storage logic
    complex pure computation
```

Do not introduce an FFI boundary when a small amount of ordinary Kotlin code is clearer and lower risk.

## 2. Security objective

The objective of Rust adoption is to reduce classes of memory-safety and state-management defects in code that benefits from strong ownership/type guarantees.

Rust does **not** eliminate:

- authorization or permission mistakes;
- logic errors;
- insecure product requirements;
- UI spoofing or confusing consent flows;
- cryptographic design errors;
- protocol misuse;
- privilege escalation caused by incorrect Android integration;
- unsafe FFI contracts;
- vulnerable third-party dependencies;
- maintenance/update failures.

A Rust implementation still requires threat modeling, tests, dependency review, runtime validation, and appropriate Android security boundaries.

## 3. Prefer Rust for high-value risk domains

Rust is strongly preferred when practical for new Sable-owned code involving:

- attacker-controlled or malformed input parsing;
- binary formats, archive/file parsing, metadata parsing, codecs, or serialization;
- network/protocol/state-machine logic owned by Sable;
- cryptographic verification and integrity-checking logic using reviewed libraries;
- security-sensitive storage transformations;
- deterministic rule engines and complex domain logic;
- native services or long-lived native components where memory safety materially reduces risk;
- complex algorithms that can be isolated from Android framework state;
- components that benefit substantially from fuzzing/property testing;
- reusable cores intended to run across Android and host tests.

## 4. Prefer Kotlin/Compose for Android integration

Kotlin/Compose remains the default for:

- application UI;
- accessibility semantics;
- Activity/Fragment/Service lifecycle;
- permission request UX;
- Android roles/default-app integration;
- Intent handling;
- PackageManager and LauncherApps integration;
- ContentProvider/MediaStore/Calendar provider integration;
- Telecom framework integration;
- Camera2/CameraX integration;
- notification channels and Android notification plumbing;
- system Settings delegation;
- Compose UI tests and AndroidX UIAutomator integration.

Do not wrap mature Android framework APIs in a large JNI layer merely to increase the percentage of Rust code.

## 5. FFI boundary rules

FFI is a security boundary and must remain narrow.

Every new JNI/Binder/native boundary must document:

```text
what crosses the boundary
which side owns memory
which side validates lengths/ranges/encoding
which errors are representable
whether nullability is possible
threading assumptions
lifetime assumptions
panic/exception behavior
what unsafe code is required
how malformed input is tested
```

Prefer typed value transfer over raw pointers and shared mutable state.

Never let a panic unwind across an FFI boundary. Convert failures to an explicit error/result contract.

Do not expose an unrestricted native API surface merely for convenience.

## 6. Unsafe Rust policy

SableOS minimizes `unsafe` and treats it as review-significant code.

Every new unsafe block/function/trait implementation/raw-pointer operation or unsafe FFI contract must answer:

```text
Why is unsafe necessary?
What invariant makes it sound?
Who establishes and maintains that invariant?
What input can violate it?
Which tests/fuzz targets exercise it?
Can safe Rust or a safer library remove it?
```

Requirements:

- keep unsafe regions small and localized;
- add a nearby safety explanation when the invariant is not obvious;
- review unsafe changes independently from ordinary formatting/refactoring noise;
- track unsafe usage as a review metric, not as proof of security;
- do not accept "written in Rust" as a substitute for unsafe-boundary review.

## 7. Required Rust quality/security gates

Applicable Sable Rust code should converge on the following controls.

### 7.1 Universal source controls

```text
rustfmt check
Clippy with reviewed lint policy
unit tests for deterministic behavior
unsafe inventory/review
CodeQL Rust where extraction/support is viable
```

Treat high-confidence correctness findings as blocking unless a documented exception exists.

Do not globally deny every `pedantic`, `restriction`, `nursery`, or experimental Clippy lint without evaluating signal, stability, and codebase fit.

### 7.2 Cargo-managed components

Where Cargo is the canonical dependency/build graph, add:

```text
cargo test
cargo-audit / RustSec
cargo-deny
cargo-vet for higher-assurance third-party dependency provenance
```

Dependency policy must consider more than published vulnerabilities. License, source provenance, duplicate/banned crates, maintenance status, and audit provenance also matter.

### 7.3 Soong-owned Android Rust

Do not create a second Cargo dependency graph solely to satisfy Cargo-oriented security tools when the production component is canonically built by Soong.

For Soong-owned Rust:

- use the pinned Android/AOSP Rust toolchain;
- use tree-supported rustfmt/Clippy paths where available;
- preserve Soong dependency/source identity;
- add host/test harnesses only when they represent the same production logic honestly;
- do not claim `cargo-audit` coverage for dependencies that are not actually represented by a canonical Cargo lockfile.

## 8. Fuzzing and property testing

Fuzzing is required for selected high-risk Rust code rather than every crate.

High-priority fuzz targets include:

- archive and file parsers;
- media/metadata parsers owned by Sable;
- message/protocol parsers;
- serialization/deserialization boundaries;
- FFI input decoders;
- state machines processing untrusted events;
- security/integrity token parsing;
- migration/import formats.

Use `cargo-fuzz`/libFuzzer for Cargo-managed targets where appropriate. For larger continuous fuzzing, dedicated CI such as ClusterFuzzLite may be introduced later rather than consuming developer-workstation storage by default.

Fuzz corpora/crash artifacts are security evidence and should be stored outside production source trees.

## 9. Miri and sanitizers

Use Miri selectively for suitable pure-Rust/unsafe code when a compatible toolchain exists. It is supplemental and does not need to be a universal PR gate.

For native/FFI components, available sanitizer/instrumented test configurations may complement Rust checks when they accurately exercise production boundaries.

Tool incompatibility with the pinned Android toolchain should be recorded as a limitation rather than worked around by silently changing production compiler/toolchain assumptions.

## 10. Privacy-first application rule

New Sable applications should default to the least data and privilege necessary.

Unless requirements explicitly justify otherwise:

```text
network permission          absent
location permission         absent
contacts/calendar/media     absent until feature requires it
usage access                absent
accessibility service       absent
background execution        minimized
analytics/tracking SDKs      absent
cloud account dependency     absent
```

A Rust core must not be used to disguise unnecessary data collection or broad Android privileges.

Local/offline functionality is preferred where it satisfies the product need.

## 11. Inherited application decision model

Do not replace an inherited Android/GrapheneOS-derived application merely because SableOS wants more Rust code or stronger branding.

For every proposed replacement, answer:

1. What concrete security, privacy, maintenance, UX, or architectural problem does replacement solve?
2. What privileges and Android roles will the replacement require?
3. Which attack surface and update obligations move to SableOS?
4. Why is Rust materially beneficial for the risky portions?
5. Which Android-facing portions should remain Kotlin/platform code?
6. What unsafe/FFI boundaries will exist?
7. What deterministic/property/fuzz tests will cover the risky core?
8. Which third-party dependencies become SableOS's security responsibility?
9. How will upstream vulnerabilities and compatibility changes be tracked?
10. What is the fallback/rollback plan if the replacement is not yet equivalent?

No replacement is accepted merely because the new implementation compiles or has fewer lines of code.

## 12. Candidate matrix

This table expresses initial architectural direction, not automatic authorization to implement or replace a component.

| Component | Initial direction | Rust role |
| --- | --- | --- |
| **Sable Calculator** | strong early Sable-owned candidate | deterministic arithmetic/domain core where useful; Kotlin/Compose UI |
| **Sable Notes** | strong candidate | storage/model/import/export core; Kotlin/Compose UI |
| **Files / archive handling** | hybrid candidate | parsers, archive/file operations and validation where Sable owns them |
| **Gallery/media metadata** | hybrid candidate | metadata/index parsing and deterministic transformations |
| **Messaging** | high-complexity hybrid | parser/protocol/domain core may benefit strongly; Android messaging/role/provider integration remains platform/Kotlin-facing |
| **Contacts** | limited Rust benefit initially | optional domain/import logic; Android contacts/provider integration remains Kotlin/platform-facing |
| **Dialer / Phone** | retain proven implementation first | Rust only for isolated non-Telecom domain logic if later justified |
| **Camera** | retain proven implementation first | Rust only for isolated processing algorithms; Camera2/CameraX and permissions remain Android-facing |
| **Clock / Alarm** | Kotlin generally adequate | Rust only if complex deterministic logic justifies FFI cost |
| **App store / updater** | security-critical; evaluate separately | Rust may benefit integrity/protocol logic, but update trust/privilege design dominates language choice |
| **Auditor / attestation** | security-critical; retain/evaluate separately | Rust may benefit crypto/parsing only after threat-model and compatibility review |
| **PDF/document parser** | security-critical; do not casually rewrite | Rust can reduce memory-safety risk, but parser correctness/fuzzing/update ownership remain substantial |
| **Browser / WebView** | retain hardened browser/WebView substrate | no Sable browser-engine rewrite merely to use Rust |
| **Android Settings** | retain platform implementation | Sable may provide bounded entry points, not duplicate platform plumbing |
| **SystemUI / Keyguard** | retain platform implementation unless separately justified | not an application-level Rust migration target |
| **Accessibility/TalkBack-class service** | retain proven platform/accessibility implementation first | language choice secondary to accessibility correctness and privileged integration |

## 13. Replacement sequencing

The preferred progression is from low-privilege/deterministic components toward higher-risk integrations only after the application architecture and testing model is proven.

```text
Calculator
    -> Notes / similarly bounded offline utility
    -> Files/Gallery data-processing cores
    -> selected Messaging parsing/domain components
    -> other bounded utilities
    -> evaluate privileged/security-critical apps individually
```

Browser engines, core platform Settings, Keyguard/SystemUI, and complex security-critical privileged components are not default rewrite targets.

## 14. SableStart precedent

SableStart may use a small Rust core while retaining Kotlin/Compose for launcher lifecycle, Android package/profile APIs, permissions, and UI.

This is the intended pattern: Rust can own deterministic/security-sensitive logic without attempting to replace Android's launcher APIs or Compose UI framework.

SableStart must remain the Sable product HOME/UI while Launcher3 Quickstep remains the underlying recents/gesture provider unless a separately approved architecture change supersedes that boundary.

## 15. Testing architecture

For a hybrid Rust/Kotlin application, tests should be layered:

```text
Rust unit/property/fuzz tests
    pure core behavior and malformed input

Kotlin/host tests
    Android-facing adapters where practical

Compose UI Test
    deterministic first-party UI semantics

AndroidX UIAutomator
    cross-app/system boundary behavior

shell evidence gates
    exact artifact binding, package state, permissions/AppOps,
    logs, screenshots and evidence sealing
```

No one layer substitutes for the others.

## 16. Dependency trust

Prefer small, well-maintained dependencies with clear provenance and narrow purpose.

For a new Rust dependency, review at least:

- source and ownership;
- license compatibility;
- maintenance/release activity;
- transitive dependency growth;
- unsafe usage where material;
- advisory history;
- feature flags/default features;
- whether an existing platform/standard-library capability is sufficient;
- whether the dependency needs network/build-script behavior;
- audit/vet status for high-assurance code.

Avoid adding a dependency solely to save a few lines in security-sensitive code.

## 17. Build-system rule

The production build graph remains authoritative.

If an Android Rust component is Soong-owned, Soong is the production source/dependency truth. If a standalone Sable utility is intentionally Cargo-managed, Cargo's lockfile and dependency policy become canonical for that component.

Do not maintain two divergent dependency graphs for the same production artifact.

## 18. Evidence before replacing an inherited app

Before a Sable replacement becomes a required product application, evidence should include as applicable:

```text
exact old/new source identity
old/new package/component/role mapping
permission/privilege comparison
dependency and unsafe inventory
static analysis results
unit/property/fuzz results
artifact hashes and manifest inspection
runtime compatibility matrix
privacy/network behavior
migration/interoperability behavior
accessibility validation
rollback/fallback path
security-update ownership
```

Replacing a mature inherited component creates a long-term security maintenance commitment; the acceptance evidence must reflect that fact.

## 19. Decision rule

When choosing between Kotlin, Rust, hybrid, or retaining an inherited component, prefer the option that yields the best combination of:

```text
least privilege
smallest understandable attack surface
memory safety where it matters
clear Android framework ownership
strong deterministic testing
fuzzability of untrusted-input code
maintainable dependency provenance
fast upstream security response
accessible and correct user behavior
simple rollback
```

The desired outcome is not the maximum amount of Rust. It is **a security- and privacy-oriented SableOS codebase whose language boundaries are deliberate, reviewable, testable, and proportionate to risk**.
