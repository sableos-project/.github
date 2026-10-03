# SableOS default application and replacement policy

Status: **current normative policy — 2026-10-02**

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

The frozen Panther image historically used standalone
`org.sableos.launcher` / SableLauncher at that accepted milestone.

Current Sable **first-party** HOME architecture is newer:

```text
SABLE_FIRST_PARTY_HOME=Launcher3QuickStep hosting Sable Start
STANDALONE_SABLELAUNCHER_RUNTIME=RETIRED
THIRD_PARTY_HOME_SELECTION_ALLOWED=YES
FORCE_SABLE_HOME_AFTER_USER_SELECTION=NO
```

The historical Panther image is not rewritten by this later architecture
decision.

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

Keyboard-first devices move toward the common Sable Camera / Camera Control
Deck and Sable Keyboard/input stack, but that does not retroactively reopen the
frozen Panther image.

## Default versus user-selected application

A Sable first-party/factory default is not a lock on user choice.

```text
THIRD_PARTY_HOME_INSTALL_ALLOWED=YES
THIRD_PARTY_HOME_SELECTION_ALLOWED=YES
THIRD_PARTY_IME_INSTALL_ALLOWED=YES
THIRD_PARTY_IME_ENABLE_ALLOWED=YES
THIRD_PARTY_IME_SELECTION_ALLOWED=YES
FORCE_SABLE_HOME_AFTER_USER_SELECTION=NO
FORCE_SABLE_IME_AFTER_USER_SELECTION=NO
```

Third-party launchers do not automatically receive privileged Quickstep,
SystemUI or Private-Space integration. Third-party IMEs do not automatically
inherit Sable's direct-boot/critical-entry qualification.

## Product composition ownership

Common application selection/integration belongs in `vendor_sable`.

Device repositories may add only bounded target-specific exceptions. A
device-specific workaround must not duplicate the common application family.

## Privilege

Default/replacement status does not justify broader privilege.

New privileged permissions, signer-based access, system-only APIs or special
SELinux domains require explicit architecture/security evidence.

## Sable Hub replacement boundary

Sable Hub is common product functionality, not a Panther-only optional app.
Titan-family integration must preserve Panther V1 Connected Apps semantics:
generic package+user discovery/configuration, Android
notification/conversation ingestion, source-authorized RemoteInput reply,
Open-app fallback and bounded local derived history.

Provider-specific private protocols, credentials, private-database scraping and
embedded provider WebViews are not substitutes for this architecture. Provider
names such as WhatsApp, Signal, Telegram and LinkedIn remain compatibility and
evidence targets rather than a hard-coded allowlist.

Sable Messages replacement/integration work must not silently replace or weaken
Sable Hub.

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
