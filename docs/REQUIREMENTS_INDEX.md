# SableOS requirements index

Status: **current index — 2026-09-24**

This index points to the current requirement owners. Historical milestone files
are retained in their repositories but are not current authority when they
conflict with these documents.

## Program status and execution

- `CURRENT_RELEASE_STATUS.md` — exact current state.
- `DEVELOPMENT_RELEASE_PLAN.md` — active execution order.
- `DOCUMENTATION_STATUS.md` — current vs historical classification.
- `CI_TRUST_ARCHITECTURE.md` — trust/build/artifact/deployment separation.
- `SECURITY_QUALITY_ENGINEERING.md` — security/quality evidence model.
- `SOURCE_OWNERSHIP_AND_PUBLICATION.md` — private/public source transition.
- `LICENSING.md` — publication/reuse licensing policy.

## Common product architecture

Owned by `sableos-project/platform_sable`:

- `docs/ARCHITECTURE.md`
- `docs/DEVICE_SUPPORT_LEVELS.md`
- `docs/PORTABILITY_RULES.md`
- `docs/RELEASE_MODEL.md`
- `docs/CAMERA_ENHANCEMENT_MODEL.md`
- `docs/KEYBOARD_DEVICE_TOOLS.md`

Current device roles:

```text
panther       REFERENCE_FROZEN
titan2        PORTABILITY / N0 active research
titan2-elite  PORTABILITY candidate / N0 pending
q27           RESEARCH
```

## Launcher

Current user-facing HOME:

```text
org.sableos.launcher / SableLauncher
```

Launcher3QuickStep is retained for Recents/Overview/task/gesture substrate and
is not HOME eligible.

`packages_apps_SableStart` is presentation/history reference material. Its
R3-R6 HOME-adoption/migration documents are historical.

## Product composition

Owned by `sableos-project/vendor_sable`:

- `docs/DEFAULT_APPLICATION_COMPOSITION.md`
- `docs/OWNERSHIP_BOUNDARY.md`

The accepted Panther reference includes the current Sable application family:
Launcher, Calculator, Sudoku, Minesweeper, 2048, Media, Reader, Text Reader,
Hub/Messages, Mail, Weather and Calendar.

Reader and Text Reader are separate product identities.

## Build / artifacts / deployment

Owned by `sableos-project/build`:

- `docs/MULTI_DEVICE_ENGINEERING_INTERFACE.md`
- `docs/CI_EXECUTION_MODEL.md`
- `docs/MILESTONE_EVIDENCE_GATES.md`
- `docs/REPRODUCIBILITY.md`
- `docs/AUTHORIZATION_MODEL.md`
- `docs/ANDROID_PRODUCT_BUILD_PLAYBOOK.md`

K1/K2 establishes registry schema v2 plus common-policy/device-transport
deployment separation. Serial is required only for device contact.

## Source composition

Owned by `sableos-project/platform_manifest`:

- `docs/DEVELOPMENT_MILESTONE_COMPOSITION.md`
- `docs/MANIFEST_HIERARCHY.md`
- `docs/RELEASE_MANIFEST_POLICY.md`
- `docs/SOURCE_COMPOSITION_MODEL.md`

Release composition records exact source plus exact external artifact inputs
when used. Artifact kind is explicit.

## Panther

Owned by `sableos-project/device_sable_panther`.

Panther is a frozen accepted reference. Its R7 daily-driver matrix is historical
requirements/evidence context, not an active pending checklist.

## Keyboard-first work

The active common-product design work covers launcher focus/navigation,
type-to-search, shortcuts/commands, square layouts, Sable Keyboard/input and
Sable Camera.

Titan research should feed device adapters rather than fork common apps.

## Historical R8 material

R8 build-review, pre-image and application-train documents are preserved for
audit/provenance. Their pending states and old execution sequence must not be
used as current project status.
