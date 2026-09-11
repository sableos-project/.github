# Contributing to SableOS

SableOS uses evidence-driven development and explicit repository ownership boundaries.

## Choose the correct repository

- common application behavior -> application repository such as `packages_apps_SableStart`;
- common semantic/service behavior -> `platform_sable`;
- common Android product integration -> `vendor_sable`;
- target-specific integration -> `device_sable_<target>`;
- source composition -> `platform_manifest`;
- host bootstrap/build/validation -> `build`;
- architecture/ADRs/project roadmap -> `sableos`.

Do not copy common code into a device repository to solve a target-specific problem without first reviewing the abstraction boundary.

## Change discipline

A pull request should state:

- purpose and ownership boundary;
- exact files/components affected;
- source/build/device mutation required;
- tests and evidence completed;
- claims that are proven and claims that remain open;
- portability impact on other Android/device substrates.

## Validation

Compilation is necessary but not sufficient. Depending on scope, validation may include static guards, host tests, module builds, artifact inspection, device/runtime checks, negative security tests, and recovery/update behavior.

## Mainline

`main` is the canonical active development line. Long-lived milestone branches must not become the only location of validated source or documentation. Exact validated OS compositions belong in revision-pinned manifests.
