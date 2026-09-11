# Contributing to SableOS

SableOS uses evidence-driven development, requirements-first implementation, and explicit repository ownership boundaries.

## Read requirements before coding

Before implementing a milestone, begin with:

- [`docs/REQUIREMENTS_INDEX.md`](docs/REQUIREMENTS_INDEX.md);
- [`docs/DEVELOPMENT_RELEASE_PLAN.md`](docs/DEVELOPMENT_RELEASE_PLAN.md);
- the milestone-specific requirements in the owning repository;
- the relevant architecture/ownership/validation documents.

If a product-semantic choice is not defined, do **not** silently invent one in source code or a test script. Update the owning requirements first.

Examples of decisions that require explicit documentation include:

- what counts as an All Apps entry;
- greeting time buckets/text;
- default Phone/Messaging/Browser/Camera selection;
- new permissions or privileged roles;
- theme/customization behavior;
- Calculator arithmetic semantics;
- new repository/service ownership;
- device-specific versus common implementation placement.

Implementation convenience does not override recorded requirements.

## Choose the correct repository

- common application behavior -> application repository such as `packages_apps_SableStart`;
- common semantic/service/design behavior -> `platform_sable`;
- common Android product/default-app integration -> `vendor_sable`;
- target-specific integration/qualification -> `device_sable_<target>`;
- source composition -> `platform_manifest`;
- host bootstrap/build/validation -> `build`;
- current organization-wide roadmap/policy -> `.github` while the future central `sableos` repository transition is incomplete;
- future central architecture/ADRs/project roadmap -> `sableos` after that transition.

Do not copy common code into a device repository to solve a target-specific problem without first reviewing the abstraction boundary.

Do not place an application implementation in `vendor_sable` or `platform_sable` merely to avoid creating/using the correct canonical app repository.

## Change discipline

A pull request should state:

- milestone/requirement being satisfied;
- purpose and ownership boundary;
- exact files/components affected;
- source/build/device mutation required;
- new permission/role/dependency implications;
- product composition/default-app impact;
- tests and evidence completed;
- claims that are proven and claims that remain open;
- portability impact on other Android/device substrates;
- explicit non-goals when scope could otherwise expand.

If implementation reveals a requirement needs to change, update the requirement explicitly rather than rewriting the meaning of the gate after the fact.

## Validation

Compilation is necessary but not sufficient. Depending on scope, validation may include static guards, host tests, module builds, artifact inspection, device/runtime checks, user-visible interaction evidence, negative security tests, and recovery/update behavior.

Validation tooling enforces product decisions; it does not define them. A gate should not invent semantics that are missing from the owning requirements.

Use `sableos-project/build/docs/MILESTONE_EVIDENCE_GATES.md` for the current R5–R10+ evidence model.

## Authorization discipline

State-changing validation should explicitly separate authorization for:

- source mutation;
- build/output mutation;
- network access;
- device contact;
- install/uninstall;
- reboot;
- clean/clobber/delete;
- root/remount/slot/wipe operations;
- Git commit/push/history changes.

Authorization for one class does not imply authorization for another.

## Mainline

`main` is the canonical active development line. Long-lived milestone branches must not become the only location of validated source or documentation. Exact validated OS compositions belong in revision-pinned manifests.

Internal `R*` milestones are development/validation checkpoints, not semantic SableOS product versions.