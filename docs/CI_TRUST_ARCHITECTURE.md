# SableOS CI trust architecture

Status: **normative CI/build/device/signing trust model.**

SableOS deliberately separates disposable source/application qualification from trusted Android product builds, physical-device mutation and release signing.

## 1. Current infrastructure roles

During the R8 builder/storage transition:

```text
GitHub hosted     = T0 disposable/untrusted application + static/security CI
ai-g732           = intended T1 sable-builder-01 for trusted Android/product builds
thinkpad-p50      = legacy/reference build host and historical evidence source
Pixel 7 / panther = T2 sable-device-01
OptiPlex          = T3 sable-signer-01
```

`ai-g732` becomes the active trusted Android builder only after its storage/source/tool/output environment passes a migration/preflight seal. Until then, "intended builder" is the correct claim.

The ThinkPad P50 remains useful for historical evidence comparison and bounded read-only reference work. It is not the planned R8 full-image host merely because existing OUT trees are present.

## 2. T0 — disposable GitHub application qualification

GitHub-hosted runners may execute pull-request code and fetch ordinary public build dependencies. They must not possess production signing material, device secrets or access to a persistent trusted AOSP workspace.

T0 is the normal R8 application-development authority for:

- repository/policy/workflow checks;
- Rust formatting, Clippy and unit/property tests;
- Rust dependency/advisory/provenance checks where Cargo is canonical;
- Kotlin/JVM unit tests;
- Gradle Android application compilation;
- Android Lint/static analysis;
- Compose/instrumentation/emulator tests where configured;
- exact pinned upstream Reader/Text Reader checkout and deterministic Sable adaptation;
- qualification APK generation;
- manifest/package/permission/exported-component inspection;
- native ABI/library inventory;
- APK SHA-256 and uploaded qualification artifacts;
- CodeQL/MobSF/SARIF and similar non-secret static/security jobs where appropriate.

A green T0 job proves only the stated standalone source/application claim. It never proves SableOS product selection, image membership, runtime role/default status or device behavior.

## 3. T1 — trusted Android/product builder

The trusted Android builder consumes only explicitly accepted source identities and exact frozen application artifacts/inputs.

It is used for:

- Android/Soong product-integration proof;
- narrow framework/platform/native builds where standalone Gradle cannot prove the required property;
- prebuilt/import module validation;
- PRODUCT_OUT/product-selection proof;
- target-files/image build work;
- clean reconstruction when required;
- final unsigned/engineering product artifacts for later device/signing stages.

T1 must **not** be the everyday compiler for ordinary R8 app iteration. A failure that can be detected with Cargo/Gradle/T0 belongs in Process A.

### 3.1 ai-g732 activation gate

Before `ai-g732` is treated as `sable-builder-01`, record at minimum:

```text
host identity
OS/toolchain prerequisites
filesystem and new storage identity
source repository identities
workspace/output/evidence roots
free-space floor and monitoring policy
network/build authorization policy
expected target product/release/variant/Build ID
artifact-freeze input location and hashes
```

Do not silently reuse path assumptions from the old ThinkPad workspace.

### 3.2 Trusted-source rule

Do not run arbitrary PR heads on T1. T1 input must be an exact reviewed/trusted commit, a reviewed merge candidate whose identity is explicitly approved, or an exact frozen artifact accompanied by its accepted source/workflow provenance.

## 4. T1 application-integration boundary

For sealed standalone APKs, the trusted builder proves the missing Android-product properties:

```text
sealed APK
 -> declared import/module
 -> selected product
 -> concrete PRODUCT_OUT path
 -> installed-files/target-files membership
 -> image membership
```

The exact Android 17 / GrapheneOS mechanism must be proved before it is normalized. `android_app_import` remains a candidate, not an assumption.

If the product build rebuilds an application instead of consuming the frozen artifact, that is a different integration model and its dependency/source identity must be documented explicitly.

## 5. T2 — device lab

`sable-device-01` consumes an exact hash-identified product/application artifact from a trusted stage.

Device access and mutation remain separately authorized. Permission to run CI or build code does not imply permission to:

- install/uninstall packages;
- flash/update the device;
- reboot;
- change roles/default apps;
- change network/radio state;
- root/remount;
- change slot;
- wipe userdata/metadata;
- modify SIM/eSIM/carrier configuration.

Runtime evidence binds back to exact package/image/source/artifact identity.

## 6. T3 — signing appliance

`sable-signer-01` is not a general GitHub Actions runner and does not execute untrusted application or PR code.

It accepts only an approved release candidate with expected source/manifest/artifact hashes, verifies those identities locally, signs using protected production material and emits signing/output provenance.

Production signing material must not be present on GitHub-hosted runners or the ordinary trusted builder.

## 7. R8 workflow separation

Current R8 qualification should remain split into independently diagnosable lanes such as:

```text
Rust correctness
Rust dependency/security
Android compile/tests
Android static analysis
Reader compile/tests
Reader policy/static
Text Reader compile/tests
Text Reader policy/static
APK artifact seal
repository/workflow policy
```

Do not hide a failed static/security/policy gate behind a successful APK compile job.

A future release aggregator may require all accepted lane results, but the underlying checks should remain separable.

## 8. Upstream/reuse-source trust

External/first-party reuse repositories are checked out by exact commit for an accepted qualification run.

Rules:

- branch names alone are not freeze identities;
- deterministic Sable flavor/patch/overlay logic is version-controlled;
- third-party dependencies retain their own provenance/license/security obligations;
- an upstream repo being owned by the same developer does not make every transitive dependency trusted;
- the accepted APK records both upstream/source identity and Sable adaptation/workflow identity.

## 9. Workflow supply-chain rules

Organization workflows should converge on:

- explicit least-privilege `permissions`;
- third-party Action pinning to immutable commit SHA for trusted/release-critical jobs;
- explicit timeouts;
- concurrency cancellation for replaceable PR jobs;
- diagnostic artifacts/logs on failure where useful;
- no secrets exposed to untrusted fork code;
- no mutable cross-repository workflow reference in a trusted/release path;
- explicit dependency/tool versions where practical;
- no silent network access in a stage whose declared policy is offline/no-fetch.

## 10. Cache policy

- T0 caches are untrusted convenience data.
- T0-produced artifacts become integration candidates only after their exact identity/provenance is sealed and accepted.
- T1 build caches remain isolated from arbitrary PR runners.
- clean/reconstruction gates must be able to bypass or invalidate mutable caches when the claim requires it.
- signing does not trust a cache to establish artifact identity; it trusts verified hashes/provenance.

## 11. Release trust chain

Preferred high-level chain:

```text
reviewed source / pinned upstream
        |
        v
T0 standalone qualification
        |
        v
exact application artifact freeze
        |
        v
T1 product integration + Android image build
        |
        v
T2 device acceptance
        |
        v
approved release candidate
        |
        v
T3 signing
```

Every transition records the exact input/output identity. A later stage must not substitute a differently rebuilt artifact without reopening the appropriate provenance gate.

## 12. Claim discipline

Examples:

```text
Gradle APK compile PASS
    != SableOS image inclusion PASS

product import rule PASS
    != runtime package/role PASS

Panther runtime PASS
    != all-device release support

unsigned image PASS
    != signed release provenance PASS
```

Trust tiers exist to keep those claims separate and auditable.