# SableOS organization documentation status

Status: **current documentation map for `sableos-project`.**

This index exists because SableOS preserves milestone requirements/evidence as historical records while the architecture continues to evolve. A document can remain technically valid history while no longer defining the next work item.

Use these classes:

```text
CURRENT_NORMATIVE   defines current architecture/policy/requirements
ACTIVE_WORKSTREAM   requirements/status for work currently in progress
HISTORICAL          preserved earlier milestone requirement/evidence/reference
REVIEWED_NO_CHANGE  audited and still consistent with current architecture
```

Do not rewrite historical evidence merely to make it resemble the current plan.

## `.github`

| Document | Class | Current meaning |
| --- | --- | --- |
| `profile/README.md` | CURRENT_NORMATIVE | public organization landing/current program summary + gate badges |
| `docs/CURRENT_RELEASE_STATUS.md` | CURRENT_NORMATIVE | exact current R9 gate state, local-CI model and Panther/Titan sequence |
| `docs/DEVELOPMENT_RELEASE_PLAN.md` | CURRENT_NORMATIVE | milestone/build/release direction |
| `docs/REQUIREMENTS_INDEX.md` | CURRENT_NORMATIVE | read-before-code index |
| `docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md` | CURRENT_NORMATIVE | organization R8 app qualification/integration policy |
| `docs/RUST_APPLICATION_ARCHITECTURE.md` | CURRENT_NORMATIVE | Rust/Kotlin/FFI ownership policy |
| `docs/DEFAULT_APP_AND_REPLACEMENT_POLICY.md` | CURRENT_NORMATIVE | app adoption/default/replacement policy |
| `docs/CI_TRUST_ARCHITECTURE.md` | CURRENT_NORMATIVE | GitHub/builder/device/signer trust model |
| `docs/LICENSING.md` | CURRENT_NORMATIVE | organization licensing-status note; per-component until an explicit repo-wide license is adopted |
| `CONTRIBUTING.md` | CURRENT_NORMATIVE | contribution/requirements/evidence expectations |
| `PULL_REQUEST_TEMPLATE.md` | CURRENT_NORMATIVE | PR claim/ownership/validation template |
| `SECURITY.md` | REVIEWED_NO_CHANGE | private reporting and development security scope remains valid |

## `platform_sable`

| Document | Class | Current meaning |
| --- | --- | --- |
| `README.md` | CURRENT_NORMATIVE | common platform/application architecture entry point |
| `docs/ARCHITECTURE.md` | CURRENT_NORMATIVE | common-vs-Android-vs-device ownership model |
| `docs/RELEASE_MODEL.md` | CURRENT_NORMATIVE | semantic release/support/provenance model |
| `docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md` | ACTIVE_WORKSTREAM | R8-A first design contract; Follow system/Light/Dark + bounded accent |
| `docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md` | ACTIVE_WORKSTREAM | detailed R8 app architecture |
| `docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md` | ACTIVE_WORKSTREAM | R8-D2 Reader TXT/share/TTS/OCR contract |
| `docs/R9_SABLE_UTILITY_APP_MODEL.md` | HISTORICAL / SUPERSEDED MILESTONE LABEL | useful application-model guidance; Calculator milestone assignment moved to R8-B |
| `docs/R9_CALCULATOR_REQUIREMENTS_DRAFT.md` | ACTIVE_REQUIREMENTS_SOURCE / SUPERSEDED MILESTONE LABEL | unresolved Calculator semantics remain relevant; app now belongs to R8-B |
| `docs/PORTABILITY_RULES.md` | REVIEWED_NO_CHANGE | common/device portability rules remain valid |
| `docs/DEVICE_SUPPORT_LEVELS.md` | REVIEWED_NO_CHANGE | Panther PRIMARY/support-level model remains valid |

## `packages_apps_SableStart`

| Document | Class | Current meaning |
| --- | --- | --- |
| `README.md` | CURRENT_NORMATIVE / STATUS | launcher source-line and active PR status |
| `docs/ARCHITECTURE.md` | CURRENT_NORMATIVE | launcher-specific ownership; must consume shared R8-A contract |
| `docs/HOME_ADOPTION_GATE.md` | REVIEWED_NO_CHANGE | HOME/default-role adoption remains separate from feature compilation |
| `docs/MIGRATION_STATUS.md` | HISTORICAL + CURRENT STATUS HEADER | preserves R3-R5 source/build evidence; should point to newer branch/PR state |
| `docs/R6_ALL_APPS_AND_GREETING.md` | HISTORICAL REQUIREMENTS | preserved R6 requirement baseline |
| R3/R6 evidence/status documents | HISTORICAL | do not rewrite past evidence |

The active launcher direction is R9 Launcher3/Quickstep foundation + Sable Start presentation. Historical standalone-HOME and R8 customization branches remain evidence/reference; current product HOME is `com.android.launcher3/.sable.SableQuickstepLauncher`.

## `vendor_sable`

| Document | Class | Current meaning |
| --- | --- | --- |
| `README.md` | CURRENT_NORMATIVE / STATUS | common product integration boundary |
| `docs/DEFAULT_APPLICATION_COMPOSITION.md` | CURRENT_NORMATIVE | qualified artifact -> product/image/default rules |
| `docs/OWNERSHIP_BOUNDARY.md` | REVIEWED_NO_CHANGE with minor integration clarification | common product composition, never copied app source |

Open R6 product-composition PRs remain implementation state and must not be described as merged `main` content until actually integrated.

## `device_sable_panther`

| Document | Class | Current meaning |
| --- | --- | --- |
| `README.md` | CURRENT_NORMATIVE / STATUS | Panther adapter/qualification role and next R8 image gate |
| `docs/VALIDATION_MODEL.md` | CURRENT_NORMATIVE | Process B/runtime/evidence model |
| `docs/R7_DAILY_DRIVER_VALIDATION.md` | HISTORICAL/ONGOING REQUIREMENTS | R7 daily-driver matrix remains valid for unclosed runtime claims |
| `docs/PORTING_BOUNDARY.md` | REVIEWED_NO_CHANGE | Panther-specific code only when evidence shows target-specific need |

## `build`

| Document | Class | Current meaning |
| --- | --- | --- |
| `README.md` | CURRENT_NORMATIVE / STATUS | build-tooling role and host transition |
| `docs/CI_EXECUTION_MODEL.md` | CURRENT_NORMATIVE | Process A disposable CI vs Process B trusted product build |
| `docs/ANDROID_PRODUCT_BUILD_PLAYBOOK.md` | CURRENT_NORMATIVE | product build/provenance ladder including qualified APK integration |
| `docs/MILESTONE_EVIDENCE_GATES.md` | CURRENT_NORMATIVE | R5-R7 historical gates + current R8 Process A/B closure |
| `docs/BUILD_LAYOUT.md` | CURRENT_NORMATIVE | source/output/evidence/tool separation; paths host-configurable |
| `docs/AUTHORIZATION_MODEL.md` | REVIEWED_NO_CHANGE | separate operation authorization remains valid |
| `docs/REPRODUCIBILITY.md` | REVIEWED_NO_CHANGE with artifact-input extension | reproducibility must include sealed external APK inputs when used |
| `docs/CI_STATUS_20260911.md` | HISTORICAL SNAPSHOT | preserve; not current CI status |

## `platform_manifest`

| Document | Class | Current meaning |
| --- | --- | --- |
| `README.md` | CURRENT_NORMATIVE / STATUS | OS source composition + external qualified artifact provenance boundary |
| `docs/DEVELOPMENT_MILESTONE_COMPOSITION.md` | CURRENT_NORMATIVE | milestone composition, now including R8 sealed APK inputs |
| `docs/SOURCE_COMPOSITION_MODEL.md` | CURRENT_NORMATIVE | source projects plus explicitly tracked non-source artifacts |
| `docs/RELEASE_MANIFEST_POLICY.md` | CURRENT_NORMATIVE | immutable release source/artifact input identity |
| `docs/MANIFEST_HIERARCHY.md` | REVIEWED_NO_CHANGE | hierarchy remains structurally valid |
| `docs/R5_R3_RECONSTRUCTION_PLAN.md` | HISTORICAL ACTIVE-UNTIL-CLOSED | preserved R5 reconstruction plan; not the current R8 roadmap |

## Current cross-repository architecture

```text
Process A: local direct qualification
  controlled build machine
      -> tests/static/security/app builds
      -> source-bound evidence
      -> exact artifact freeze

Process B: OS product integration
  platform_sable contracts
  + vendor_sable product selection
  + device_sable_panther bounded adapter
  + platform_manifest source/artifact provenance
  + build trusted orchestration
      -> ai-g732 Panther image
      -> sable-device-01 runtime campaign
      -> sable-signer-01 only for approved release candidates
```

## Historical-document rule

A historical file should receive a short supersession/status note only when a reader could reasonably mistake it for current direction. Do not alter old PASS/FAIL data, hashes, command results or original acceptance boundaries simply because the program moved forward.

## Update rule

When architecture changes:

1. update the owning normative document;
2. update `DEVELOPMENT_RELEASE_PLAN.md` if milestone/build/release sequencing changed;
3. update `REQUIREMENTS_INDEX.md` if reading paths or major workstreams changed;
4. update this map when a document changes class;
5. update repository README/status pages;
6. leave historical evidence intact and link the superseding current document.