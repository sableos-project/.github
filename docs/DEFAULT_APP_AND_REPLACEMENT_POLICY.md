# Default application and replacement policy

Status: **current normative policy — 2026-09-24**

SableOS replaces an inherited application because the replacement has a better
qualified product/security/UX boundary, not merely because Sable branding exists.

## Evidence ladder

A replacement decision distinguishes:

```text
source qualified
  != trusted artifact
  != product selected
  != image/artifact membership
  != default role
  != runtime acceptance
```

Each layer is proven separately.

## Accepted Panther reference

The frozen Panther R9 reference includes the accepted Sable application family:

```text
SableLauncher
Calculator
Sudoku
Minesweeper
2048
Media
Reader
Text Reader
Hub / Messages
Mail
Weather
Calendar
```

Current HOME is standalone `org.sableos.launcher`. Launcher3QuickStep is
Recents/Overview/task substrate only.

Vanadium is the accepted browser reference on Panther.

Panther Camera remains the documented upstream/preprocessed presentation
exception for the frozen image.

## Reader products

Sable Reader and Sable Text Reader are distinct accepted products. Do not merge
or duplicate them merely because historical planning proposed one Reader
identity.

## Replacement requirements

Before replacing an inherited app/default, prove as applicable:

- exact source/upstream provenance;
- package/component identity;
- required permissions and privilege;
- local/network data behavior;
- lifecycle/background behavior;
- import/product selection;
- image membership;
- role/default ownership;
- upgrade/migration behavior;
- runtime functionality;
- accessibility and keyboard-first behavior where relevant;
- security/update ownership.

## Privilege

Do not add `sharedUserId`, privileged permissions, system UID or custom SELinux
authority merely to make replacement easier.

A future Sable Camera may use `SYSTEM_CAMERA` only on a device where physical
evidence proves a system-only camera capability is valuable and negative
third-party access tests preserve the boundary.

## Multi-device rule

Common app/replacement policy is device-independent.

Panther is frozen. Titan 2 and Titan 2 Elite should consume the same common
qualified app source where compatible. Device adapters may change hardware
integration, not clone application ownership.

## Artifact classes

K1 registry v2 means a product may be carried by target-files, full-device
images, GSI/system images or bounded bundles. Artifact class does not change the
replacement evidence requirements.

## Production signing

Development acceptance does not imply production release signing/update
ownership. That remains a later program.
