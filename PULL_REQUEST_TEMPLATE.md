## Milestone / requirement

Identify the milestone and exact requirement/document section this change satisfies. If the behavior was not previously specified, link the requirements update that defines it.

## Purpose

Describe what this changes and why.

## Ownership boundary

Which repository/layer should own this change, and why? Explain any common-vs-device-specific decision.

## Scope

List the affected components/files and any cross-repository dependencies.

Also state explicit non-goals when adjacent work could otherwise expand scope.

## Product / application composition impact

Does this add, remove, replace, default, or change the role of an application/package? Does it alter product package lists, roles/default handlers, overlays, or privileged allowlists?

If yes, reference the applicable default-application/product-composition decision.

## Permissions / authority / dependencies

List any new or changed:

- Android permissions/AppOps;
- privileged/system status;
- roles/default handlers;
- services/providers/Binder authority;
- network requirement;
- external/upstream libraries or services.

State `none` when there is no change.

## Authorization / mutation

State whether this work requires network access, source mutation, build output mutation, device contact, install/uninstall, reboot, Git mutation, deployment/signing, clean/clobber/delete, root/remount/slot/wipe, or other destructive operations.

Authorization for one class does not imply authorization for another.

## Validation completed

Describe static checks, deterministic tests, builds, artifact inspection, runtime/device/user-visible evidence, negative tests, and reconstruction evidence completed as applicable.

Bind important evidence to exact source/build/artifact identities.

## Claim boundary

What does this PR prove? What remains unproven, blocked, not tested, or deferred?

Do not promote compile/build success into a runtime/product claim without evidence.

## Requirements drift check

- [ ] Implementation follows the current documented requirement.
- [ ] No new product semantic was invented only inside source/test code.
- [ ] Any requirement change is documented explicitly.
- [ ] Test tooling enforces requirements rather than defining them.

## Portability impact

Describe effects on Panther, Bramble, MediaTek/QWERTY, or other future substrates where relevant.

If the change is intentionally common, explain why it does not belong in a device repository. If it is device-specific, identify the evidence that makes it target-specific.