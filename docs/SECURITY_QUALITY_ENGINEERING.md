# SableOS security and quality engineering

Status: **current normative engineering policy — 2026-09-24**

Security and quality claims are evidence claims, not branding claims.

## Claim separation

Always distinguish:

```text
source/static quality
build correctness
artifact identity
product composition
privilege/permission behavior
deployment correctness
runtime functionality
performance/power
production release security
```

A PASS in one category does not imply another.

## Current device context

```text
Panther        accepted R9 / REFERENCE_FROZEN
Titan 2        PORTABILITY N0 active research
Titan 2 Elite  independent PORTABILITY candidate
Q27            RESEARCH
```

A Panther result never automatically applies to Titan 2. A Titan 2 result never
automatically applies to Elite.

## Source quality

Applicable repositories should use:

- compiler warnings as actionable engineering data;
- format/lint/static analysis;
- deterministic unit tests;
- dependency/provenance review;
- secret/private-path scanning;
- targeted fuzz/property testing for parsers/domain code;
- no stale/dead product code in shipped artifacts where practical.

## Android application security

Prefer ordinary application sandboxing.

Every sensitive permission/privilege must have a product reason. Review:

- dangerous/runtime permissions;
- AppOps;
- exported components;
- roles/defaults;
- foreground/background services;
- notification behavior;
- backup policy;
- network permissions and remote content;
- privileged allowlists;
- custom SELinux domains/signing identities where applicable.

Do not introduce new `sharedUserId` use for convenience.

## Native code

For every shipped native library prove:

- exact source/toolchain;
- ABI;
- JNI boundary;
- final APK/ELF identity;
- 16 KiB page-size compatibility where required by target Android;
- runtime execution on representative hardware where the claim matters.

## K1 artifact security

Artifact registry v2 binds exact artifact kind and hashes.

Supported generic representation does not equal device support. Device adapters
separately gate release artifact registration.

Never use physical serial as artifact identity.

## K2 deployment security

Common deployment code owns:

- explicit authorization;
- registered-artifact verification;
- exact selected-serial binding;
- evidence sealing;
- preservation/destructive-data policy;
- bounded recovery.

Device adapters own transport/partition/AVB/restore semantics.

Panther is qualified. Titan 2, Titan 2 Elite and Q27 remain fail-closed until
their own contracts are proven.

Never generalize Panther `flashall` or slot semantics to MediaTek targets.

## Keyboard-first quality

Keyboard-first acceptance must test more than text entry:

- visible deterministic focus;
- arrow/D-pad movement;
- Enter/Space activation;
- Back/Escape;
- type-to-search;
- shortcut discoverability/conflicts;
- stable focus restoration;
- no focus traps;
- accessibility;
- square/near-square layout;
- physical keyboard event mapping;
- pointer/touch-surface behavior where present.

## Camera security

Keep sensor/HAL/app-visible/system-visible capabilities separate.

Grant `SYSTEM_CAMERA` only per device after physical proof, with negative tests
showing ordinary third-party apps cannot discover/access system-only cameras.

## Telephony and emergency safety

Telephony/IMS/SMS/MMS testing must use controlled normal endpoints. Do not use
emergency services as test targets.

## Performance / power

Measure before setting thresholds. Device performance is independent evidence.

Use representative startup/jank/input/memory/CPU/I/O/power/thermal/suspend
measurements and ratchet thresholds from known baselines rather than inventing
unsupported numbers.

## Build trust

The controlled local build host is release-critical authority. GitHub is
source/review/documentation and optional disposable source-policy CI.

Full CI may be source-bound. Source drift invalidates its attestation.

Do not automatically clean/clobber after failures. Preserve evidence and partial
outputs.

## Production signing/update

Production app keys, AVB hierarchy, OTA signing/update service, key custody,
rollback/recovery and release support lifecycle remain deferred.

N0 functional portability is not production security qualification.

## Release discipline

Use exact statuses:

```text
PASS
FAIL
BLOCKED
NOT_TESTED
UNKNOWN
```

Do not turn partial evidence into a broad security/release verdict.
