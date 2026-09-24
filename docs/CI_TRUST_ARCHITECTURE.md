# SableOS CI and trust architecture

Status: **current normative architecture — 2026-09-24**

## Trust separation

SableOS separates:

```text
source/review
local qualification
trusted standalone application builds
Android product/image builds
artifact registration
device deployment
physical acceptance
production signing/update
```

A PASS at one layer never implies a later layer.

## Current infrastructure

```text
GitHub
    source, review, issues, public documentation
    optional disposable source-policy workflows
    not release-critical Android build authority

ai-g732
    controlled local CI
    trusted Android/application build host
    release artifact/evidence registry host

physical devices
    explicitly serial-scoped runtime/acceptance targets

production signer
    not commissioned
```

The canonical engineering path is local-direct. A GitHub self-hosted runner is
not required for current release qualification.

## Current device roles

```text
Panther         REFERENCE_FROZEN / accepted R9
Titan 2         PORTABILITY N0 research/bring-up
Titan 2 Elite   independent PORTABILITY candidate
Q27             RESEARCH
```

Panther is no longer the active feature-design target.

## Source and CI gate

Ordinary source/application defects should be caught before broad Android image
work. Local full CI is source-bound and produces an attestation keyed by exact
source commit. A release image build must fail closed if the exact source lacks
its required full-CI attestation.

Do not reuse an attestation after source changes.

## Artifact trust — K1

Artifact registry schema v2 records exact device/release/build source, tool
source, artifact kind, named artifact paths/hashes/sizes, build evidence and
metadata.

Supported classes include target-files, full-device images, GSI system images,
system/product bundles and boot/recovery bundles.

Generic schema support is not permission to register an unqualified device
release artifact; the device adapter separately gates registration.

## Deployment trust — K2

The common deployment layer owns safety invariants:

- explicit authorization;
- exact selected-device serial binding;
- registered-artifact verification;
- source/tool cleanliness requirements;
- evidence collection/sealing;
- preservation/destructive-data policy;
- bounded recovery;
- post-boot return.

The device adapter owns transport/partition semantics.

Panther currently implements the qualified target-files/A-B fastboot adapter.
Titan 2, Titan 2 Elite and Q27 remain fail-closed.

## Physical acceptance

Runtime evidence is device-specific. Panther results do not imply Titan-family
results. Titan 2 results do not imply Elite results.

Acceptance may include functionality, package/role/default identity,
permissions/AppOps, JNI/native execution, UI/accessibility, input/focus,
telephony, camera, display, power and performance evidence.

## Production signing

Production APK keys, AVB key hierarchy, OTA signing/update service, key custody,
backup/recovery/rotation and signed-release handoff are deferred.

Production signing material must not be present on disposable CI or the ordinary
development builder.

## Failure discipline

Preserve evidence and partial outputs. Do not clean/clobber as an automatic
failure response. Distinguish compile/build success, packaging/product closure,
artifact registration, flash success and runtime acceptance.
