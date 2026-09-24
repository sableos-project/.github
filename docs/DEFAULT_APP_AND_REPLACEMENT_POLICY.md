# SableOS default application and replacement policy

Status: **current normative policy — 2026-09-24**

This policy separates capability ownership, product composition and replacement
decisions. A Sable-branded implementation does not become a product default
merely because it builds successfully.

## Current accepted Panther reference

The physically accepted R9 Panther product includes the current Sable
first-party family:

```text
SableLauncher
Sable Calculator
Sable Sudoku
Sable Minesweeper
Sable 2048
Sable Media
Sable Reader
Sable Text Reader
Sable Hub / Messages
Sable Mail
Sable Weather
Sable Calendar
```

Current HOME is standalone `org.sableos.launcher` / SableLauncher.
Launcher3QuickStep remains Recents/Overview/task/gesture substrate only.

Reader and Text Reader are separate products.

## Replacement rule

For any inherited/substrate application, prove replacement as a sequence:

```text
source/application qualification
  -> trusted artifact identity
  -> product selection
  -> install path / image membership
  -> role/default behavior
  -> runtime acceptance
```

A compile PASS or launcher-visible icon is never sufficient.

## Reuse over rewrite

Do not replace a proven Android/substrate implementation merely to increase
Sable branding or Rust usage.

Prefer replacement when there is a clear product/security/privacy/interaction
benefit and Sable can own the resulting maintenance/security burden.

## Current inherited/reference applications

Where no Sable replacement has been accepted, inherited applications remain
valid product components.

Examples from the frozen Panther reference include Vanadium as the browser and
the documented upstream/preprocessed Camera presentation exception.

Keyboard-first devices move toward a common Sable Camera and Sable Keyboard /
input stack, but that does not retroactively reopen the frozen Panther image.

## Product composition ownership

Common application selection/integration belongs in `vendor_sable`.

Device repositories may add only bounded target-specific exceptions. A
device-specific workaround must not duplicate the common application family.

## Privilege

Default/replacement status does not justify broader privilege.

New privileged permissions, signer-based access, system-only APIs or special
SELinux domains require explicit architecture/security evidence.

## Multi-device rule

Panther is REFERENCE_FROZEN.

Titan 2 and Titan 2 Elite should consume the same common application source and
product semantics where compatible. Keyboard-first presentation is an
interaction profile, not a per-device application fork.

Q27 remains research-only.

## Artifact/import semantics

The accepted Panther R9 release proved its own Android 17/product integration
mechanism.

Future substrates and artifact kinds must independently prove module/import,
signing, partition, dexpreopt/uses-library and runtime semantics rather than
assuming Panther behavior.

K1 registry schema v2 can represent multiple artifact classes, but generic
registry support is not replacement/default-app qualification.

## Production signing

Development/test signing is separate from production application/AVB/OTA
signing. Production signing remains deferred.
