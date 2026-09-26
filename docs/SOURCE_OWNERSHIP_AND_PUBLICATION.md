# Source ownership and publication transition

Status: **active policy — 2026-09-25**

## Decision

Do not move the private SableOS integration monorepo wholesale into the public
organization.

During the keyboard-first transition:

```text
private integration repository
    exact release composition
    source-bound qualification/build/deployment tooling
    pre-publication experiments
    private evidence references

sableos-project
    canonical public reusable source
    common architecture/contracts
    public device adapters when mature
    public build/reconstruction tooling when publication-ready
```

The long-term goal is that a SableOS build can be reconstructed from public
source repositories plus explicitly identified private/proprietary binary inputs
and local evidence. When that is true, the private integration repository should
shrink substantially or become archival/release-only.

Public source must not be treated as publication-ready merely because code has
been copied out of the private integration repository. It also needs build,
signing, verification and installation documentation. The detailed gate is
maintained in [Source publication build and signing gate](SOURCE_PUBLICATION_BUILD_SIGNING_GATE.md).

## Why not publish the monorepo now

The integration repository currently mixes several concerns:

- reusable Sable application code;
- Android framework/product patch orchestration;
- build-host paths and local evidence conventions;
- upstream/reused source composition;
- device-specific experimental work;
- release and flash policy.

Publishing that entire tree would preserve accidental coupling rather than
establish clean public ownership boundaries.

## Publication gate per component

A component moves to `sableos-project` only when:

1. source/upstream provenance is explicit;
2. applicable license and notices are understood and publishable;
3. secret/private-path/device-serial/raw-evidence review is clean;
4. no unreviewed proprietary firmware or vendor diagnostic data is included;
5. independent build/test instructions exist;
6. signing and verification boundaries are documented;
7. public docs state whether the component is dev-signed, release-candidate
   signed, production-signed or not publicly reproducible;
8. the destination repository has a clear ownership boundary;
9. the private integration manifest/build pins the public revision;
10. parity is proven before the private duplicate is retired.

A repository may exist publicly before this gate is complete, but it must be
labeled as research, documentation, historical evidence or
`NOT_PUBLICATION_READY_SOURCE`; it must not imply that users can reproduce an
accepted SableOS image.

## Preferred public decomposition

```text
platform_sable
    semantic/design/interaction contracts

vendor_sable
    common product composition + permission/product integration

build
    reusable CI/build/artifact/deployment framework

device_sable_<target>
    bounded target-specific integration

application repositories
    SableLauncher, Camera, Keyboard, Hub, Media, Reader, etc.
```

Avoid creating a public device repository before there is real device-owned
source to maintain. Research-only notes may stay in research repositories until
the device crosses into PORTABILITY implementation.

## Private evidence boundary

Do not publish by default:

- physical device serials;
- private logs containing account/user data;
- raw stock firmware or OTA payloads unless redistribution is explicitly
  permitted;
- unreviewed vendor diagnostics;
- credentials, signing material or host secrets;
- production signing keys, release signing keys or private key-management
  procedures;
- evidence paths that imply a public artifact exists when it does not.

## Migration order

```text
1. documentation + common contracts
2. reusable build/device-adapter framework
3. build/sign/verify documentation for each moved component
4. common applications and design libraries
5. keyboard-first Camera / Keyboard components
6. mature device adapters
7. manifest/release composition
8. retire redundant private copies only after parity
```
