# SableOS CI trust architecture

Status: **normative CI/build/device/signing trust model.**

## Named infrastructure

```text
thinkpad-p50      = sable-builder-01
optiPlex          = sable-signer-01
Pixel 7 / panther = sable-device-01
GitHub hosted     = untrusted/disposable CI
```

The role names are security boundaries, not merely hostnames.

## T0 — GitHub-hosted, untrusted/disposable

GitHub-hosted runners may execute pull-request code because they are disposable and must not contain Sable production secrets or persistent trusted state.

Allowed work:

- repository/policy checks;
- workflow/YAML and shell syntax checks;
- XML/JSON parsing;
- full-SHA action-pin verification;
- secret/private-key heuristics;
- CodeQL and source-oriented security scans;
- deterministic pure unit tests;
- future R6 greeting/search/order tests;
- future R8 theme-policy tests;
- future R9 Calculator-engine tests;
- SARIF and failure-report generation.

Default permission is `contents: read`; jobs add only the minimum required permission such as `security-events: write` for SARIF/CodeQL.

T0 must not receive production signing material, personal credentials, writable canonical AOSP workspaces, or ADB access to Sable devices.

## T1 — sable-builder-01

The ThinkPad P50 remains the trusted AOSP/Soong build and reconstruction host.

It may perform exact revision acquisition, `platform_manifest` reconstruction, Soong/module/product builds, artifact inspection, source-before/source-after integrity seals, reproducibility experiments, and evidence generation.

The strongest CI rule is:

> A GitHub pull request may cause arbitrary code execution only on disposable infrastructure. Persistent Sable build, device, and signing infrastructure executes only explicitly trusted source identities.

Unreviewed PR/fork code must never run directly on `sable-builder-01`.

The current developer/reference workspace must not become a generic Actions scratch tree. Automated work should move to a dedicated account and layout such as:

```text
/srv/data/sable-ci/
    source/
    mirrors/
    workspaces/
    out/
    evidence/
    runner/
```

A future `sable-ci` account should have no sudo by default, no production signing material, no personal GitHub/SSH credentials, no default ADB access, and no write access to the developer reference workspace unless a documented gate explicitly needs it.

Trusted Android builds should split acquisition and build:

```text
ACQUISITION: network allowed only as required -> fetch exact revisions -> verify -> seal
BUILD:       network denied -> compile -> inspect -> hash -> seal evidence
```

The ThinkPad has already shown that tested unprivileged bubblewrap namespace modes are blocked by host policy. CI must not weaken host security merely to make sandboxing convenient. Prefer the dedicated account/filesystem boundary and, when practical, disposable KVM build VMs.

## Device boundary — sable-device-01

The Panther test device consumes an already-built artifact with an exact hash. Device automation may verify the hash, perform an explicitly authorized install/update, launch exact components, inspect package/launcher inventory, permissions/AppOps, collect bounded logs/screenshots, and run semantic UI smoke tests.

Calls/SMS/MMS remain semi-automated initially because carrier/external-endpoint behavior must be observed separately.

Device testing is not general PR execution and is not release signing.

## T2 — sable-signer-01

The OptiPlex is the highly trusted release-signing machine.

It is a signing appliance, not a development or general CI machine. It must not run ordinary Actions jobs, arbitrary PR/build scripts, or the normal AOSP tree.

Intended flow:

```text
approved artifact + exact source/manifest identity + SHA-256
        -> sable-signer-01 verifies identity
        -> production signing
        -> signed artifact + checksums + signing provenance
```

Normal signing posture should be offline, encrypted, narrowly administered, and powered off when not needed. Production signing material is never placed on GitHub-hosted runners or the ordinary CI builder.

## Artifact trust flow

```text
GitHub-hosted T0 checks
        -> reviewed/trusted source identity
        -> sable-builder-01
        -> artifact + SHA-256 + evidence
        -> sable-device-01 runtime qualification
        -> release approval
        -> sable-signer-01
        -> signed release + provenance
```

Every downstream tier verifies the expected source/build identity and cryptographic hash before accepting an artifact.

## GitHub Actions requirements

Sable workflows must use:

- third-party actions pinned to full commit SHA;
- explicit least-privilege `permissions`;
- timeouts;
- concurrency cancellation for superseded PR jobs where appropriate;
- diagnostic artifacts on failure;
- SARIF upload for supported security scanners;
- reviewable dependency/action update PRs;
- reusable Sable workflows referenced by exact commit SHA across repositories.

`vaachak-platform/vaachak-mobile` is a CI/security design reference for full-SHA action pinning, least privilege, CodeQL, MobSF/SARIF, failure artifacts, concurrency controls, Dependabot, and semantic UI smoke-test identifiers. Sable adopts those patterns, not Vaachak-specific Gradle, application-security, or signing assumptions.

## Cache boundary

Untrusted PR/T0 caches must never become trusted AOSP or release input. Trusted Android caches should be scoped by exact substrate/toolchain/source identity. Release/reproducibility gates must be able to run without mutable PR caches.

## Rollout

- **C1 Fast PR CI:** GitHub-hosted policy/static/security/pure tests.
- **C2 Trusted builder:** `sable-builder-01`, exact trusted SHA, component/AOSP build and evidence.
- **C3 Reconstruction:** fresh `platform_manifest` checkout, acquisition/build network split, product evidence.
- **C4 Device lab:** `sable-device-01`, explicitly authorized runtime validation.
- **C5 Release:** `sable-signer-01`, approved inputs only, isolated signing and provenance.

## Change-control questions

Before moving any task to a higher-trust machine, document: what code executes, what persistent state/credentials are visible, whether devices can be reached, how the exact source identity was approved, what may be mutated, and how evidence is sealed. If those answers are unclear, keep the job disposable or manual.
