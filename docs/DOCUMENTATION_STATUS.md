# SableOS documentation status

Status date: **2026-09-25**

This index classifies public documentation so historical milestone evidence is
not mistaken for current product architecture.

Current deep-dive reconciliation record: `docs/DOCUMENTATION_AUDIT_20260925.md`.
Prior reconciliation record: `docs/DOCUMENTATION_AUDIT_20260924.md`.

## Current authority chain

```text
.github/docs/CURRENT_RELEASE_STATUS.md
  -> aimindseye/sableos CURRENT_STATUS.md while private integration remains release authority
  -> accepted ADR-0010 / ADR-0011 in the private integration repository
  -> platform_sable architecture/support/portability/release docs
  -> build multi-device/evidence contracts
  -> vendor_sable common product composition
  -> per-device adapter docs
  -> historical evidence only when investigating prior claims
```

## Current release authority

```text
R9_PANTHER_HUB_V1_CLOSURE=MERGED_PR_110
R9_PANTHER_IMAGE_SOURCE=edf62e5bb08372a1395841d6cc5d78d3148a7695
R9_PANTHER_TARGET_FILES_SHA256=a0b359613c4f30e9a834fba212e0b044a97d63ed0537c59471c31b99b627d285
R10_KEYBOARD_FIRST_DESIGN_V1=MERGED_PR_108
PANTHER_ROLE=FROZEN_TOUCH_FIRST_REFERENCE
TITAN2_ROLE=ACTIVE_KEYBOARD_FIRST_N0_TARGET
```

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
| `profile/README.md` | CURRENT_STATUS |
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
| `docs/DOCUMENTATION_AUDIT_20260924.md` | HISTORICAL_EVIDENCE / AUDIT_RECORD |
| `docs/DOCUMENTATION_AUDIT_20260925.md` | CURRENT_STATUS / AUDIT_RECORD |

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
presentation/history reference, not the current HOME package authority.

All R3/R5/R6 migration/runtime/HOME-adoption files are
`HISTORICAL_EVIDENCE` or `HISTORICAL_SUPERSEDED`.

## vendor_sable

- `README.md` — CURRENT_STATUS / CURRENT_NORMATIVE.
- `docs/DEFAULT_APPLICATION_COMPOSITION.md` — CURRENT_NORMATIVE.
- `docs/OWNERSHIP_BOUNDARY.md` — CURRENT_NORMATIVE.

## device_sable_panther

- `README.md` — CURRENT_STATUS.
- `docs/PORTING_BOUNDARY.md` — CURRENT_NORMATIVE.
- `docs/VALIDATION_MODEL.md` — CURRENT_NORMATIVE frozen-reference model.
- `docs/R7_DAILY_DRIVER_VALIDATION.md` — HISTORICAL_EVIDENCE.

Panther is REFERENCE_FROZEN, not PRIMARY. The current Panther reference image is
bound to `edf62e5b` and target-files SHA-256 `a0b359...`.

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

## Open issue policy

Remaining open issues in the private integration repository are intentionally
left open until Titan 2 SableOS install closure. They represent active Titan
qualification, keyboard-first platform work and Panther/Titan-shared polish
follow-ups rather than stale R8/R9 broad blockers.
