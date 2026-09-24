# SableOS security, quality, performance and test engineering

> **Current execution overlay — 2026-09-24:** Panther is a frozen accepted reference; Titan 2 / Titan 2 Elite are independent N0 portability targets. K1/K2 now separates generic artifact identity from adapter-owned deployment transport. Security/quality evidence must preserve that separation and must not treat a booting GSI as production qualification.


Status: **normative engineering-assurance policy.**

SableOS is built around privacy, least privilege, narrow trust boundaries, reproducible evidence and measurable behavior. Security is not treated as a marketing claim or as something supplied by one language, one scanner or one build stage. Quality, performance, testability and provenance are part of the same engineering system.

The goal is not to claim that SableOS is "secure by inspection." The goal is to make important claims reviewable, testable, reproducible and difficult to bypass accidentally.

## 1. Core engineering principles

SableOS development follows these rules:

- preserve mature Android sandboxing, permission, SELinux, verified-boot and framework boundaries unless there is a demonstrated reason to change them;
- apply least privilege to applications, services, build infrastructure and CI credentials;
- default new Sable applications to no network, location, contacts, calendar, accessibility-service or other sensitive authority unless a requirement explicitly justifies it;
- use Rust where memory safety, deterministic state, parsing, protocol/state-machine logic or complex pure computation materially reduce risk;
- keep Android lifecycle, permissions, framework/provider APIs, accessibility, Media3/MediaSession and other platform-facing integration in Kotlin/Android where that is the safer ownership boundary;
- keep JNI/Binder/native boundaries narrow, typed and explicitly tested;
- do not create new shared UIDs, privileged status, custom SELinux domains or broad permissions for convenience;
- pin accepted source, dependency, toolchain and artifact identities at trust-boundary transitions;
- separate compilation, static analysis, trusted artifact production, product integration, image construction and physical runtime acceptance;
- optimize from measurements, not anecdotes;
- treat tests, coverage, static analysis, fuzzing, benchmarks and device evidence as complementary controls rather than interchangeable proof.

The governing Rust rule remains:

```text
RUST_BY_RISK, NOT_RUST_BY_BRANDING
```

## 2. Security framework

Application security requirements are aligned to the OWASP Mobile Application Security Verification Standard (MASVS), supported by the OWASP Mobile Application Security Testing Guide (MASTG) and current Mobile Top 10 / MASWE guidance.

SableOS uses this as a requirements/evidence framework, not as a claim that a scanner can certify the product automatically.

Relevant areas include:

```text
storage and local data
cryptography and key use
authentication/authorization where applicable
network behavior and transport security
Android platform interaction
code quality and update safety
privacy and data minimization
resilience/tamper controls only where the product requires them
```

Each applicable control should map to source review, automated checks, runtime/device evidence, or an explicit documented exception.

## 3. Layered test architecture

No single test layer closes an application or product claim.

```text
Rust unit/property/fuzz tests
    deterministic core behavior, malformed input, state machines

Kotlin/JVM tests
    adapters and non-device Android-facing logic

Android/Gradle compile + lint/static analysis
    application integration and Android correctness

Compose UI tests
    deterministic Sable-owned UI semantics

AndroidX UIAutomator / instrumentation
    cross-app, role, permission and system-surface behavior

trusted standalone artifact qualification
    exact APK/DEX/JNI identity, permissions and native compatibility

Soong/product/image qualification
    exact product integration and filesystem/image evidence

physical Panther/Titan campaigns
    boot, runtime, JNI, UX, performance and regression evidence
```

Manual acceptance is recorded as manual evidence and is not silently relabeled as automated coverage.

## 4. Static analysis and code-quality controls

### Already used in the SableOS repositories

Current repositories already contain or have demonstrated parts of this control set:

- Rust `rustfmt`, Clippy and unit tests;
- `cargo-audit` / RustSec advisory checks;
- Android Gradle compilation and Android Lint;
- package/manifest/permission inspection and APK SHA-256 sealing;
- CodeQL Java/Kotlin in Sable-owned Android source repositories;
- MobSF/mobsfscan source scanning in Sable-owned Android source repositories;
- repository policy checks including syntax validation and rejection of high-risk credential-container files;
- policy requiring third-party GitHub Actions to be pinned to immutable full commit SHAs in repositories using the reusable fast-policy workflow;
- 16 KiB native ELF/APK compatibility checks for accepted R8 native libraries;
- trusted image/package/device evidence gates with explicit mutation/authorization boundaries.

### Required consolidation for the private canonical pipeline

The private implementation pipeline is being consolidated to include:

```text
CodeQL Java/Kotlin
CodeQL Rust where supported
Android Lint
detekt
ktlint
MobSF/mobsfscan static analysis
Gitleaks or equivalent secret scanning
rustfmt
Clippy with reviewed lint policy
cargo-audit
cargo-deny
unsafe Rust inventory/review
Sable-specific Semgrep/policy rules where they add signal
```

Tool findings are triaged by severity, exploitability, reachability and architectural context. Suppressions/exceptions require documented rationale; disabling a scanner globally to obtain a green build is not an accepted fix.

## 5. Dependency and supply-chain controls

For Cargo-managed code:

```text
Cargo.lock is source-controlled where the application dependency graph is canonical
cargo-audit checks RustSec advisories
cargo-deny checks advisory/license/source/duplicate/banned dependency policy
cargo-vet may be added for higher-assurance provenance as dependency graphs stabilize
```

For Gradle/Android code:

- use wrapper/tool versions bound to the accepted application build;
- use dependency locking/verification for accepted dependency graphs;
- review repositories, licenses and transitive-dependency growth;
- avoid adding a dependency solely to save a small amount of security-sensitive code;
- use Dependabot or equivalent update discovery without automatically promoting updates into trusted artifacts.

For GitHub Actions:

- default workflow permissions are least-privilege/read-only unless a job explicitly requires more;
- external Actions are pinned to immutable full commit SHAs in the hardened pipeline;
- mutable caches are convenience data and do not establish trusted provenance.

Trusted A2/product artifacts record source/tool/dependency identity and artifact hashes. SBOM/provenance output is a near-term requirement for the private canonical pipeline.

## 6. Coverage policy

Coverage is a signal, not a substitute for meaningful tests.

Planned canonical reporting:

```text
Kotlin/JVM       -> Kover
Rust             -> cargo-llvm-cov / LLVM source coverage
instrumentation  -> Android/AGP/JaCoCo-compatible device coverage where useful
```

Coverage rollout uses a ratchet model:

1. generate and retain reports for every relevant PR;
2. establish a trustworthy baseline;
3. prevent unexplained regressions;
4. require new deterministic/security-sensitive code to have corresponding tests;
5. introduce module-specific thresholds only after the baseline is representative.

Calculator/conversion/game Rust cores can reasonably target high deterministic coverage. Raw line coverage is less meaningful for framework glue, Compose rendering and generated Android code, so those layers additionally rely on semantic UI/instrumentation/runtime tests.

## 7. Fuzzing, property testing, Miri and sanitizers

Fuzzing is targeted by risk rather than applied mechanically to every crate.

High-priority fuzz/property targets include:

- parsers and import formats;
- untrusted metadata and serialized inputs;
- JNI/FFI decoders and boundary validation;
- protocol/state-machine transitions;
- security/integrity token parsing;
- deterministic calculator/conversion and game-state invariants where property testing is useful.

`cargo-fuzz` / libFuzzer is the preferred Cargo-managed path. Larger continuous fuzzing such as ClusterFuzzLite may be introduced where it provides enough signal to justify operational cost.

Miri and sanitizers are selective supplemental controls for suitable unsafe/native code. Tool incompatibility with an accepted Android toolchain is recorded as a limitation rather than hidden by changing the production compiler solely to satisfy a tool.

## 8. Performance engineering

Performance is measured on supported hardware and treated as a regression dimension, not inferred from language choice.

Relevant measurements include, as applicable:

```text
cold/warm application startup
frame time / jank / missed frames
input-to-render latency
Rust domain-operation latency
CPU time and wakeups
memory PSS/RSS / allocations
APK and native-library size
I/O behavior
battery/power impact for sustained/background workloads
full-build and incremental-build behavior where infrastructure changes are involved
```

Use Android Macrobenchmark/Baseline Profiles and platform tracing/metrics where appropriate, plus focused Rust benchmarks for pure domain code. Panther and Titan 2 performance are measured independently; a Panther result is not silently generalized to different hardware/form factors.

Thresholds are introduced from measured baselines and ratcheted. Optimization that weakens correctness, privacy, accessibility or security requires explicit review rather than being accepted solely for benchmark improvement.

## 9. Privacy and permission review

Every new application/capability or material change must answer:

```text
What data does it read, derive, store or transmit?
What Android permissions/AppOps/roles does it need?
What components are exported and why?
Does it require network access?
What third-party SDK/library receives data?
Can the capability remain local/offline?
What survives uninstall/profile deletion/reset?
What user-visible consent/control exists?
```

No analytics/tracking SDK is introduced by default. Network-backed behavior is explicit product scope rather than inherited convenience.

## 10. Trusted CI/build separation

SableOS separates trust levels:

```text
T0 GitHub-hosted CI
    disposable PR qualification; no production signing material or trusted AOSP workspace

T1 ai-g732 trusted builder
    accepted exact source/tool identities; A2/B1/B2/B3 development artifacts

T2 physical Panther/Titan devices
    exact hash-bound deployment and runtime acceptance

later signing environment
    separately commissioned; outside ordinary development CI
```

A self-hosted runner on the trusted builder must not execute arbitrary pull-request code. Runner accounts are isolated, non-sudo where practical, without production signing material and without unnecessary device/USB/secrets access.

Persistent private source/runner storage requires an explicit at-rest protection policy. Infrastructure hardening is tracked as a prerequisite for declaring the private canonical build host fully commissioned.

## 11. Reproducibility and artifact provenance

The accepted claim ladder is:

```text
source identity
 -> dependency/toolchain identity
 -> tests/static/security results
 -> trusted standalone artifact
 -> Soong/product integration
 -> target-files/image
 -> physical runtime
```

Every transition records exact input/output identity. Failed evidence is preserved rather than deleted to make a later run appear clean.

Fresh OUT_DIR reconstruction is required to prove repeatability; historical successful OUT directories remain evidence rather than becoming hidden dependencies. Panther and Titan 2 use isolated outputs and, where compatible, the same common Sable application artifacts/product composition.

## 12. Current implementation status versus required next work

Documentation must not claim a control is enforced simply because it is planned.

### Demonstrated/implemented in at least part of the current SableOS estate

```text
Rust fmt/Clippy/tests
cargo-audit
Android compile/tests
Android Lint
CodeQL Java/Kotlin in Sable-owned Android repositories
MobSF/mobsfscan in Sable-owned Android repositories
Dependabot for GitHub Actions in existing repositories
reusable workflow syntax/credential/action-pin policy
APK/permission/component/hash qualification
native/JNI/16 KiB compatibility qualification
trusted artifact/image/device evidence gates
explicit production-signing separation
```

### Required infrastructure consolidation before the next large implementation expansion

```text
private canonical source reconciliation
protected review/required-check policy for canonical main
action/toolchain pinning across the private pipeline
CodeQL Java/Kotlin + Rust consolidation
MobSF consolidation
detekt + ktlint
Gitleaks/equivalent secret scan
cargo-deny
Kover + cargo-llvm-cov coverage reports and ratchet
OWASP MASVS/MASTG evidence mapping
selected fuzz/property targets
Gradle dependency verification/locking
SBOM/provenance output
trusted self-hosted runner isolation
persistent-source at-rest protection decision
fresh source-bound reconstruction when a claim requires it
```

### Selective/later assurance controls

```text
cargo-vet
Miri/sanitizer jobs
continuous fuzzing infrastructure
module-specific hard coverage floors
performance-regression budgets after stable baselines
Titan 2 and Titan 2 Elite independent hardware performance/portability gates
```

## 13. Claim discipline

The following statements are intentionally not equivalent:

```text
scanner clean
    != vulnerability free

coverage high
    != behavior correct

Rust code
    != secure code

APK builds
    != trusted artifact

trusted artifact
    != integrated image

image boots
    != runtime/product acceptance

Panther passes
    != Titan 2 passes
    != Titan 2 Elite passes

Panther + Titan development passes
    != production release/signing closure
```

SableOS engineering documentation should state the strongest claim actually supported by evidence and preserve known limitations beside it.
