# SableOS licensing

Status: **current licensing policy note; not a substitute for per-repository or
per-file license text.**

SableOS is a multi-repository Android platform project that combines Sable-owned
work with AOSP/GrapheneOS-derived platform code and selectively reused
third-party application/components.

## Current organization state

As of 2026-09-24, the public `sableos-project` repositories do **not** declare
one organization-wide repository license. The organization README therefore uses:

```text
License: per-component
```

That badge is descriptive, not a new license grant.

## Rules

- Existing upstream or reused code retains its applicable license, copyright
  notices and attribution requirements.
- File-level or component-level license headers remain authoritative where
  present.
- A repository without an explicit license file must not be assumed to be MIT,
  Apache-2.0 or another permissive license merely because related Android code
  uses that license.
- New third-party integrations must record source/provenance and applicable
  licensing obligations before product adoption.
- A future organization-wide default license for original Sable-owned code must
  be an explicit project decision and should be added as a real repository
  license before the README badge is changed to a specific SPDX license.

## Why the badge is intentionally conservative

SableOS consumes components from multiple ecosystems and license families. A
single attractive badge such as `MIT` or `Apache-2.0` would be misleading
until the project formally chooses and applies such a license to the applicable
Sable-owned repositories.

Repository and component license inventories should remain part of supply-chain
and release provenance.
