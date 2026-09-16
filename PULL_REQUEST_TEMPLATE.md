## Milestone / requirement

Identify the current milestone/workstream and exact requirement/document section this change satisfies. If behavior was not specified, link the requirements update that defines it.

## Execution class

Choose the primary class and explain any crossover:

- [ ] Process A — standalone application/source qualification
- [ ] Process B — SableOS product/image/device integration
- [ ] architecture/policy/documentation only
- [ ] historical evidence/status only

## Purpose

Describe what this changes and why.

## Ownership boundary

Which repository/layer owns this change, and why? Explain any application-vs-platform-vs-product-vs-device decision.

## Scope / non-goals

List affected components/files and cross-repository dependencies. State explicit non-goals where adjacent work could expand scope.

## Source / upstream / artifact identity

Record exact identities as applicable:

```text
source repository + commit
upstream/reuse repository + commit
qualification workflow/run
APK/native artifact SHA-256
package/application ID
ABI/native-library set
manifest/permission/component inventory
```

State `not applicable` only when this PR genuinely has no source/artifact identity boundary.

## Product / application composition impact

Does this add, remove, replace, default, import, or change the role of an application/package? Does it alter product package lists, roles/default handlers, overlays, privileged allowlists, install partition/path, target-files/image expectations, or release manifest inputs?

If yes, reference the applicable product-composition/default-application decision.

A standalone qualified APK is not automatically a product/default app.

## Permissions / authority / dependencies

List new or changed:

- Android permissions/AppOps;
- privileged/system status;
- exported activities/services/providers/receivers;
- roles/default handlers;
- Binder/service/provider authority;
- network/model-download requirement;
- storage/media/data access;
- external/upstream libraries, models, data or services;
- native/JNI/FFI boundaries.

State `none` when there is no change.

## Authorization / mutation

State whether this work requires network access, source mutation, build-output mutation, Android build execution, device contact, install/uninstall, reboot, role/default changes, Git mutation, deployment/signing, clean/clobber/delete, root/remount/slot/wipe, or other destructive operations.

Authorization for one class does not imply authorization for another.

## Validation completed

Describe the applicable independent gates, for example:

```text
Rust fmt/clippy/tests/security
Kotlin/JVM tests
Gradle APK build
Android lint/static analysis
JNI/native ABI packaging
Reader/upstream pin tests
APK package/manifest/permission inspection
product selection / PRODUCT_OUT / target-files / image proof
runtime/device/user-visible evidence
reconstruction/reproducibility evidence
```

Bind important evidence to exact source/build/artifact identities.

## Integration freeze impact

For R8 application work:

- [ ] does not change a frozen input
- [ ] changes an application candidate that is not yet frozen
- [ ] intentionally replaces a previously frozen input and reopens downstream integration evidence
- [ ] not applicable

Explain the exact artifact/source identity that product integration should consume.

## Claim boundary

What does this PR prove? What remains `UNPROVEN`, `BLOCKED`, `NOT_TESTED`, deferred or intentionally out of scope?

Do not promote compile/build success into product/image/runtime/release claims without evidence.

## Requirements drift check

- [ ] Implementation follows the current documented requirement.
- [ ] No still-TBD product semantic was silently selected in source/test code.
- [ ] No new product semantic exists only in implementation.
- [ ] Any requirement/architecture change is documented explicitly.
- [ ] Test tooling enforces requirements rather than defining them.
- [ ] Historical evidence was not rewritten to fit the new implementation.

## Portability / host impact

Describe effects on Panther, future device targets, Android/substrate versions, and build hosts where relevant.

If the change is common, explain why it does not belong in a device repository. If device-specific, identify the evidence that makes it target-specific.

For trusted Android build changes, state whether the change depends on the `ai-g732` builder transition or on historical ThinkPad-only state.

## Rollback / fallback

For product/default/replacement/build-system changes, identify the rollback or previously proven fallback. State `not applicable` only for changes where rollback semantics genuinely do not apply.