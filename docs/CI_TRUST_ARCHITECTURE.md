# SableOS CI trust architecture

> **Current execution mode (2026-09-20):** GitHub-hosted build/qualification
> workflows are no longer the authoritative CI path, and no GitHub self-hosted
> runner is active. SableOS runs direct local CI on the controlled build machine,
> reusing the canonical pinned host toolchain and writing source-bound evidence.
> GitHub remains source/review/issues/documentation infrastructure. The historical
> T0 hosted-runner model below is retained as trust-model background and a possible
> future disposable lane, not current release authority.
>
> Canonical current status: [CURRENT_RELEASE_STATUS.md](CURRENT_RELEASE_STATUS.md).


Status: **normative CI/build/device/signing trust model.**

SableOS separates disposable qualification, trusted standalone application builds, trusted Android product/image builds, physical-device validation and later production signing.

The detailed security/code-quality/coverage/performance policy is defined in [`SECURITY_QUALITY_ENGINEERING.md`](SECURITY_QUALITY_ENGINEERING.md).

## 1. Current infrastructure roles

```text
GitHub             = source hosting / review / issues / documentation
local controlled CI= active source/static/app qualification
ai-g732            = trusted development/build host
GitHub self-hosted = disabled / deferred pending encryption + isolation
thinkpad-p50       = historical/reference; future signing-host candidate only
Pixel 7 / panther  = primary R9 runtime target
Titan 2            = queued keyboard-first portability/runtime target
```

There is no active `sable-signer-01` yet. OptiPlex is not part of the current signing plan.

`ai-g732` becomes the active trusted builder only after its storage/source/tool/output environment passes migration/preflight sealing and the private-source/runner hardening requirements are closed. The ThinkPad remains a reference/evidence host until the build migration is complete and may later be commissioned as an offline signing appliance, but that production role is deferred.

## 2. T0 — disposable A1 qualification

GitHub-hosted runners may execute pull-request code and ordinary public dependency resolution under workflow policy. They must not possess production signing material, device secrets or access to a persistent trusted AOSP workspace.

T0 is suitable for:

- repository/workflow policy checks;
- Rust format/Clippy/unit/property/fuzz tests;
- Cargo dependency/advisory/provenance checks;
- Kotlin/JVM tests;
- Gradle Android compilation;
- Android Lint/static analysis;
- CodeQL / SARIF-producing source analysis;
- MobSF/mobsfscan Android source analysis;
- detekt/ktlint and similar deterministic quality checks;
- Kover / cargo-llvm-cov coverage reporting;
- Compose/emulator/instrumentation tests where useful;
- pinned Reader/Text Reader qualification;
- package/manifest/permission/component inspection;
- native ABI inventory;
- qualification APK SHA-256;
- secret-scanning and dependency-policy outputs;
- static/security outputs such as SARIF.

A T0 artifact is qualification evidence, not automatically a trusted product binary.

Coverage or a green scanner result does not replace threat modeling, architecture review, fuzzing of risky boundaries or device/runtime acceptance.

## 3. T1 — trusted builder on ai-g732

T1 executes only exact accepted source identities and accepted dependency/toolchain configurations.

A self-hosted runner on T1 must not execute arbitrary pull-request code. It must be isolated from production signing material, unnecessary USB/device access and unnecessary secrets. Persistent private-source/runner storage requires an explicit at-rest protection policy before the host is considered fully commissioned.

### 3.1 A2 trusted standalone application build

A2 rebuilds accepted application source outside the AOSP product graph and produces the artifact eligible for the R8 freeze.

Record:

```text
exact source/upstream commits
Gradle/JDK/SDK/NDK/Rust identities
lockfile/provenance hashes
trusted APK SHA-256
DEX/JNI inner-content hashes
native ABI inventory
16 KiB ELF/APK compatibility
manifest/package/permission/component state
security/static-analysis summary
coverage provenance where applicable
SBOM/provenance identity where available
```

GitHub/T0 and A2 hashes may be compared where reproducible; they do not have to be blindly assumed identical across intentionally different environments.

### 3.2 B1 pre-image Android integration

T1 proves:

```text
frozen trusted APK
 -> declared Soong import/module
 -> signing/certificate processing
 -> JNI handling
 -> dexpreopt / uses-library wiring
 -> product selection
 -> PRODUCT_OUT install
```

The exact target-tree behavior is discovered and sealed rather than guessed from one generic Soong version.

### 3.3 B2/B3 image development

After B1 closes, T1 builds the accepted development image for Panther, then Titan 2 portability/integration as planned.

T1 is not the everyday compiler for ordinary R8 app iteration.

## 4. ai-g732 activation gate

Before `ai-g732` is treated as the trusted builder record at least:

```text
host identity
OS/toolchain prerequisites
filesystem/new-storage identity
persistent-source at-rest protection decision
source repository identities
workspace/output/evidence roots
isolated runner/service account policy
isolated OUT_DIR policy per target/variant
free-space floor + monitoring policy
network/build authorization policy
expected target product/release/variant/Build ID
artifact-freeze input locations + hashes
```

Do not silently reuse ThinkPad-specific absolute paths.

## 5. Native-library portability

R8 native libraries must be verified compatible with 16 KiB page-size systems.

Verification includes program-header alignment, APK native-library ZIP alignment and runtime page-size/JNI checks on accepted devices. Toolchain configuration is selected from the pinned NDK/toolchain rather than enforcing one linker flag when unnecessary.

## 6. T2 — device lab

Device stages consume exact hash-identified images/app artifacts from T1.

Device mutation remains separately authorized for install/flash, reboot, role/default changes, network/radio state, root/remount, slots and wipes.

Panther is the primary R8 runtime target. Titan 2 is the second portability target and should consume the same common R8 application artifacts and common product composition wherever compatible.

Device acceptance may include functional, permission/AppOps, JNI/native, UIAutomator, accessibility and performance-regression evidence. Panther measurements are not automatically generalized to Titan 2.

## 7. Production signing — deferred

Production signing is deliberately **not part of the R8 development critical path**.

Only after Panther and Titan 2 development qualification is satisfactory should the project commission a signing architecture covering:

```text
production application keys
AVB key hierarchy
OTA signing
sign_target_files_apks workflow
key custody/backup/recovery/rotation
artifact handoff and signed-output provenance
```

The ThinkPad P50 is the current future signing-host candidate after Android builds have migrated to `ai-g732`. Do not call it `sable-signer-01` until that role is designed, secured and commissioned.

Production signing material must never be present on GitHub-hosted runners or the ordinary trusted development builder.

## 8. Workflow separation

Keep independently diagnosable lanes such as:

```text
repository/workflow policy
Rust correctness
Rust dependency/security
Rust coverage
Rust fuzz/property targets
Android compile/tests
Android static/code-quality analysis
Android coverage
CodeQL
MobSF/mobsfscan
secret scanning
Reader compile/tests
Reader policy/static
Text Reader compile/tests
Text Reader policy/static
trusted A2 artifact build/seal
B1 Soong/import/product-wiring proof
Panther/Titan runtime and performance evidence
```

Do not hide a red security/policy/integration gate behind a successful APK compile.

## 9. Cache and source-trust rules

- T0 caches are untrusted convenience data.
- T1 caches remain isolated from arbitrary PR execution.
- trusted stages use exact accepted source/tool identities.
- reconstruction gates can bypass mutable caches when required.
- branch names alone never close provenance.
- upstream/reuse code remains subject to dependency/license/security review even when first-party owned.
- third-party GitHub Actions in the hardened pipeline are pinned to immutable full commit SHAs.
- workflow token permissions are least-privilege/read-only unless a job documents a stronger requirement.

## 10. Preferred R8 trust chain

```text
reviewed source / pinned upstream
        |
        v
T0 A1 disposable qualification
        |
        v
T1 A2 trusted standalone app build
        |
        v
exact trusted application freeze
        |
        v
T1 B1 pre-image Android integration proof
        |
        v
T1 B2 Panther development image
        |
        v
T2 Panther acceptance
        |
        v
T1 B3 Titan 2 development image
        |
        v
T2 Titan 2 portability acceptance
        |
        v
later production-signing workstream
```

Every transition records exact input/output identity. A later stage must not substitute a differently rebuilt artifact without reopening the relevant provenance gate.

## 11. Claim discipline

```text
GitHub Gradle PASS
    != trusted A2 artifact PASS

scanner PASS
    != vulnerability free

coverage high
    != behavior correct

trusted A2 APK PASS
    != Soong/product integration PASS

product import PASS
    != image membership PASS

Panther PASS
    != Titan 2 portability PASS

Panther + Titan 2 development PASS
    != production signing/release PASS
```

## 12. Canonical security/quality gate set

The private canonical pipeline is expected to converge on the following layered controls. Status is tracked separately so documentation does not claim a planned control is already enforced.

```text
Policy / workflow
  full-SHA Action pinning
  least-privilege GITHUB_TOKEN
  syntax/credential-container policy
  Gitleaks or equivalent secret scanning

Rust
  rustfmt
  Clippy
  unit/property tests
  cargo-audit
  cargo-deny
  cargo-llvm-cov
  selected cargo-fuzz targets
  unsafe inventory/review
  CodeQL Rust where supported
  cargo-vet / Miri / sanitizers selectively

Android / Kotlin
  Gradle compile + unit tests
  Android Lint
  detekt
  ktlint
  Kover
  CodeQL Java/Kotlin
  MobSF/mobsfscan
  Compose UI tests
  instrumentation/UIAutomator where useful

Mobile security
  OWASP MASVS/MASTG evidence mapping
  permission/AppOps/exported-component review
  network/data-flow/privacy review

Artifact / product
  APK/package/permission/component sealing
  DEX/JNI inner-content identity
  native ABI + 16 KiB compatibility
  SBOM/provenance output
  trusted A2/B1/B2/B3 evidence chain

Performance
  app startup / frame-jank / memory / CPU-I/O / power where relevant
  focused Rust/domain benchmarks
  Android Macrobenchmark/Baseline Profiles where useful
  ratcheted regression budgets after representative baselines exist
```

The normative rationale, rollout and distinction between implemented and required-next controls lives in [`SECURITY_QUALITY_ENGINEERING.md`](SECURITY_QUALITY_ENGINEERING.md).
