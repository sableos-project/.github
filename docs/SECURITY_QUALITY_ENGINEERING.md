# SableOS security and quality engineering

Status: **current normative engineering policy — 2026-09-24**

Security and quality claims are evidence-layered. A successful source check,
build, artifact registration, flash or single runtime test does not imply the
later claims.

## Evidence ladder

```text
source/static/unit/lint
  -> trusted application artifact
  -> product integration
  -> image/artifact build
  -> registered artifact
  -> device deployment
  -> runtime acceptance
  -> production release/security lifecycle
```

## Current device roles

```text
Panther        REFERENCE_FROZEN / accepted R9
Titan 2        PORTABILITY / N0 active research
Titan 2 Elite  independent PORTABILITY candidate
Q27            RESEARCH
```

No new PRIMARY device is currently declared.

A Panther PASS does not imply Titan 2 PASS. A Titan 2 PASS does not imply Titan
2 Elite PASS.

## Source quality

Common expectations include:

- deterministic/reviewable dependencies;
- lint/static analysis;
- unit/integration tests appropriate to the component;
- formatting/style gates;
- bounded parser/input behavior;
- explicit network/privacy behavior;
- accessibility correctness;
- localization-safe UI construction;
- no secret/private-path/raw-evidence leakage into public source.

## Android application security

Prefer normal sandboxed application APIs and user-granted permissions.

Any privileged permission, system-only API, signer relationship, custom SELinux
domain or platform integration must be narrowly justified and negative-tested.

A system application should still use ordinary user-revocable permission where
the Android model requires it.

## Native code

Native libraries require:

- exact source/dependency provenance;
- final APK/JNI identity;
- page-size/alignment compatibility for the selected platform;
- runtime execution proof on relevant targets;
- no hidden assumption that SoC family determines page size.

## Artifact security — K1

Registry schema v2 binds exact source/tool source, artifact kind and hashes.

Generic support for target-files, full images, GSI/system images, system/product
bundles and boot/recovery bundles does not make every device qualified for those
artifact kinds.

Device adapters explicitly gate release artifact registration.

## Deployment security — K2

Common deployment policy owns:

- explicit authorization;
- selected-serial exact binding;
- registered-artifact verification;
- evidence sealing;
- preservation/destructive-data policy;
- bounded failure recovery.

Device adapters own transport/partition/AVB/restore semantics.

The common driver must not silently inherit Panther A/B `flashall` assumptions.

Titan 2, Titan 2 Elite and Q27 remain fail-closed until independently
qualified.

## Restore before mutation

For a new device, do not enable automated mutation before the project can answer:

- exact stock restore source and hashes;
- bootloader / fastbootd identity;
- partition/super topology;
- snapshot/update state;
- AVB/vbmeta requirements;
- target-specific recovery path;
- data preservation or explicit wipe requirement.

## Camera security

Keep sensor capability, HAL capability, ordinary-app-visible capability and
system-camera capability separate.

Grant `SYSTEM_CAMERA` only where physical evidence proves useful hidden/system
cameras and third-party negative-discovery/access tests pass.

## Keyboard/input security

Separate common IME/text composition from physical-keyboard platform mapping.
Do not require a network-connected IME for basic setup/text entry.

Device scan codes, Fn/Sym behavior and vendor-key services must be inventoried
before Sable replaces stock keyboard-support packages.

## Performance and power

Performance claims are device-specific. Measure representative startup,
input/focus latency, frame/jank behavior, memory, CPU/I/O, power/suspend and
thermal behavior where they materially affect the product.

Ratchet thresholds from observed baselines rather than inventing unsupported
numbers.

## Reproducibility

Fresh reconstruction is required only when the claim requires freshness.
Historical successful OUT directories remain evidence, not hidden build inputs.

Record exact source, toolchain, target configuration, artifact hashes and
evidence roots.

## Production signing / update lifecycle

Production application keys, AVB hierarchy, OTA signing/update service, rollback
policy, key custody/backup/recovery/rotation and public support lifecycle remain
a later program.

N0 functional portability is not N2 production qualification.

## Public-source boundary

Do not publish device serials, user/account data, credentials, signing material,
raw proprietary firmware or unreviewed vendor diagnostics through public
repositories.

Reusable source should migrate to `sableos-project` only after provenance,
licensing and privacy review.
