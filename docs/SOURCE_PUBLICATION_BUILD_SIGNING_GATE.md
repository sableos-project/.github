# Source publication build and signing gate

Status: **active policy — 2026-09-25**

## Decision

Do **not** publish reusable SableOS source code as publication-ready unless the
same public repository set also documents how to build, verify and sign the
corresponding artifacts, or explicitly states that production signing is not yet
available.

A public source drop without build and signing instructions is incomplete. It may
be acceptable for research, documentation, or historical evidence only if it is
clearly labeled as such.

## Required before source is publication-ready

A public SableOS source component or repository is publication-ready only when it
has all applicable items below:

1. **Source provenance** — upstream origin, reused source, generated source and
   local Sable-owned source are distinguishable.
2. **Build environment** — host OS assumptions, required tools, disk/RAM
   expectations and network/offline expectations are documented.
3. **Source sync instructions** — exact manifest, branch, tags or commit pins are
   identified.
4. **Build command** — the command or script to build the component/image is
   documented, including expected target names and output paths.
5. **Artifact identity** — expected artifact kind, checksum procedure and release
   metadata are documented.
6. **Signing model** — test/dev signing, release signing and production signing
   are distinguished.
7. **Key boundary** — private keys, production keys and credentials are not
   published; the docs say which keys are required locally and which outputs
   cannot be reproduced without them.
8. **Verification path** — documented commands exist to verify the resulting
   artifact before flash/install/use.
9. **Install/flash boundary** — supported and unsupported install paths are
   explicit, including data-wipe/preserved-data behavior where relevant.
10. **Known non-reproducible inputs** — proprietary blobs, firmware, vendor
    payloads and manually acquired inputs are listed rather than silently
    implied.

## Signing policy

SableOS public source repositories must not imply that a user can reproduce the
accepted release image unless the accepted signing and proprietary-input boundary
is documented.

Use this terminology:

```text
DEV_SIGNED
    locally reproducible development/test signing only

RELEASE_CANDIDATE_SIGNED
    release-candidate process exists, but production signing is still separate

PRODUCTION_SIGNED
    production keys/process have been established and documented

UNSIGNED_OR_NOT_REPRODUCIBLE_PUBLICLY
    source is useful, but the published repository set cannot independently
    reproduce the accepted image
```

Production signing material must never be committed to public or private source
repositories.

## Documentation placement

For every public source repository that participates in image or app artifacts,
provide at minimum:

```text
README.md
    role, status, supported target(s), build summary

docs/BUILD.md
    exact local build procedure

docs/SIGNING.md
    signing model, key boundary, reproducibility limits

docs/VERIFY.md
    artifact checksum/identity/installation verification
```

A repository that lacks these files may still exist, but it must be labeled as
`NOT_PUBLICATION_READY_SOURCE` or equivalent in its README.

## Current SableOS status

As of 2026-09-25, Panther R9 Hub V1 is an accepted physical reference image, but
production signing remains deferred. Therefore public source publication should
remain incremental and gated until build/sign/verify documentation catches up to
each migrated component.
