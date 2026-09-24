# SableOS documentation status

Status date: **2026-09-24**

This index classifies public documentation so historical milestone evidence is
not mistaken for current product architecture.

Deep-dive reconciliation record: `docs/DOCUMENTATION_AUDIT_20260924.md`.

## Classification vocabulary

```text
CURRENT_NORMATIVE
CURRENT_STATUS
ACTIVE_WORKSTREAM
HISTORICAL_EVIDENCE
HISTORICAL_SUPERSEDED
RESEARCH_CONTEXT
```

## Organization .github

| Document | Class |
| --- | --- |
| `docs/CURRENT_RELEASE_STATUS.md` | CURRENT_STATUS |
| `docs/DEVELOPMENT_RELEASE_PLAN.md` | CURRENT_NORMATIVE |
| `docs/CI_TRUST_ARCHITECTURE.md` | CURRENT_NORMATIVE |
| `docs/SECURITY_QUALITY_ENGINEERING.md` | CURRENT_NORMATIVE |
| `docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md` | CURRENT_NORMATIVE |
| `docs/REQUIREMENTS_INDEX.md` | CURRENT_NORMATIVE |
| `docs/SOURCE_OWNERSHIP_AND_PUBLICATION.md` | CURRENT_NORMATIVE |
| `docs/LICENSING.md` | CURRENT_NORMATIVE |
| `docs/RUST_APPLICATION_ARCHITECTURE.md` | CURRENT_NORMATIVE |
| `docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md` | HISTORICAL_SUPERSEDED |
| `docs/R8_BUILD_PLAN_REVIEW_*.md` | HISTORICAL_EVIDENCE |
| `docs/R8_SECOND_EYE_QUESTIONS.md` | HISTORICAL_EVIDENCE |

## platform_sable

Current:
- `README.md`
- `docs/ARCHITECTURE.md`
- `docs/DEVICE_SUPPORT_LEVELS.md`
- `docs/PORTABILITY_RULES.md`
- `docs/RELEASE_MODEL.md`
- `docs/CAMERA_ENHANCEMENT_MODEL.md`
- `docs/KEYBOARD_DEVICE_TOOLS.md`

Historical/superseded milestone material:
- `docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md` — design-foundation history;
  accepted appearance semantics remain useful.
- `docs/R9_CALCULATOR_REQUIREMENTS_DRAFT.md` — historical requirements;
  Calculator is implemented in the accepted Panther product.
- `docs/R9_SABLE_UTILITY_APP_MODEL.md` — historical planning.
- `docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md` — historical R8 integration
  architecture, replaced by the current common-product/adapter model.
- `docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md` — superseded by the
  accepted Sable Reader / Sable Text Reader product split.
- `docs/ZINWA_Q27_FUTURE_PORTABILITY_NOTES.md` — RESEARCH_CONTEXT.

## packages_apps_SableStart

`README.md` and `docs/ARCHITECTURE.md` describe the current repository role:
presentation/history reference, not the current HOME package.

All R3/R5/R6 migration/runtime/HOME-adoption files are
`HISTORICAL_EVIDENCE` or `HISTORICAL_SUPERSEDED`. Current HOME authority is
`org.sableos.launcher` / SableLauncher; Launcher3QuickStep is Recents-only.

## vendor_sable

- `README.md` — CURRENT_NORMATIVE.
- `docs/DEFAULT_APPLICATION_COMPOSITION.md` — CURRENT_NORMATIVE.
- `docs/OWNERSHIP_BOUNDARY.md` — CURRENT_NORMATIVE.

## device_sable_panther

- `README.md` — CURRENT_STATUS.
- `docs/PORTING_BOUNDARY.md` — CURRENT_NORMATIVE.
- `docs/VALIDATION_MODEL.md` — CURRENT_NORMATIVE frozen-reference model.
- `docs/R7_DAILY_DRIVER_VALIDATION.md` — HISTORICAL_EVIDENCE.

Panther is REFERENCE_FROZEN, not PRIMARY.

## build

Current:
- `README.md`
- `docs/AUTHORIZATION_MODEL.md`
- `docs/BUILD_LAYOUT.md`
- `docs/CI_EXECUTION_MODEL.md`
- `docs/MILESTONE_EVIDENCE_GATES.md`
- `docs/MULTI_DEVICE_ENGINEERING_INTERFACE.md`
- `docs/REPRODUCIBILITY.md`
- `docs/ANDROID_PRODUCT_BUILD_PLAYBOOK.md`
- `gates/README.md`

Historical:
- `docs/CI_STATUS_20260911.md`
- `docs/R8_B1_COMMAND_REFERENCE.md`
- `docs/R8_BUILD_ENGINEER_REVIEW.md`
- `docs/R8_PREIMAGE_GATE.md`

## platform_manifest

Current:
- `README.md`
- `docs/DEVELOPMENT_MILESTONE_COMPOSITION.md`
- `docs/MANIFEST_HIERARCHY.md`
- `docs/RELEASE_MANIFEST_POLICY.md`
- `docs/SOURCE_COMPOSITION_MODEL.md`

Historical:
- `docs/R5_R3_RECONSTRUCTION_PLAN.md`

## Current authority chain

```text
.github/docs/CURRENT_RELEASE_STATUS.md
  -> .github/docs/DEVELOPMENT_RELEASE_PLAN.md
  -> platform_sable architecture/support/portability/release docs
  -> build multi-device/evidence contracts
  -> vendor_sable common product composition
  -> per-device adapter docs
  -> historical evidence only when investigating prior claims
```
