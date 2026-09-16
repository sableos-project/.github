# SableOS CI trust architecture

Status: **normative CI/build/device/signing trust model.**

SableOS separates disposable qualification, trusted standalone application builds, trusted Android product/image builds, physical-device validation and later production signing.

## 1. Current infrastructure roles

```text
GitHub hosted     = T0 disposable/untrusted A1 application/static/security CI
ai-g732           = intended T1 trusted A2/B1/B2/B3 application + Android builder
thinkpad-p50      = historical/reference builder during migration;
                    future production-signing-host candidate only
Pixel 7 / panther = T2 primary R8 runtime target
Titan 2           = T2 second R8 portability/runtime target
```

There is no active `sable-signer-01` yet. OptiPlex is not part of the current signing plan.

`ai-g732` becomes the active trusted builder only after its storage/source/tool/output environment passes migration/preflight sealing. The ThinkPad remains a reference/evidence host until the build migration is complete and may later be commissioned as an offline signing appliance, but that production role is deferred.

## 2. T0 — disposable A1 qualification

GitHub-hosted runners may execute pull-request code and ordinary public dependency resolution under workflow policy. They must not possess production signing material, device secrets or access to a persistent trusted AOSP workspace.

T0 is suitable for:

- repository/workflow policy checks;
- Rust format/Clippy/unit/property/fuzz tests;
- Cargo dependency/advisory/provenance checks;
- Kotlin/JVM tests;
- Gradle Android compilation;
- Android Lint/static analysis;
- Compose/emulator/instrumentation tests where useful;
- pinned Reader/Text Reader qualification;
- package/manifest/permission/component inspection;
- native ABI inventory;
- qualification APK SHA-256;
- static/security outputs such as SARIF.

A T0 artifact is qualification evidence, not automatically a trusted product binary.

## 3. T1 — trusted builder on ai-g732

T1 executes only exact accepted source identities and accepted dependency/toolchain configurations.

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
source repository identities
workspace/output/evidence roots
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
Rust correctness
Rust dependency/security
Android compile/tests
Android static analysis
Reader compile/tests
Reader policy/static
Text Reader compile/tests
Text Reader policy/static
trusted A2 artifact build/seal
B1 Soong/import/product-wiring proof
repository/workflow policy
```

Do not hide a red security/policy/integration gate behind a successful APK compile.

## 9. Cache and source-trust rules

- T0 caches are untrusted convenience data.
- T1 caches remain isolated from arbitrary PR execution.
- trusted stages use exact accepted source/tool identities.
- reconstruction gates can bypass mutable caches when required.
- branch names alone never close provenance.
- upstream/reuse code remains subject to dependency/license/security review even when first-party owned.

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

trusted A2 APK PASS
    != Soong/product integration PASS

product import PASS
    != image membership PASS

Panther PASS
    != Titan 2 portability PASS

Panther + Titan 2 development PASS
    != production signing/release PASS
```
