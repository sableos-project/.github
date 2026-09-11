# SableOS security policy

SableOS is under active development and is not yet a production-supported mobile OS release.

## Reporting security issues

Please do not disclose exploitable security issues in a public issue or discussion. Use GitHub's private security reporting / Security Advisory mechanism for the affected repository when available.

If private reporting is not yet enabled for the relevant repository, contact the project maintainers through a private channel before publishing technical details.

## Scope

Security reports are especially relevant for:

- privilege-boundary violations;
- SELinux/policy bypasses;
- unsafe Binder/AIDL/service authorization;
- verified-boot/update/signing weaknesses;
- application sandbox or permission bypasses;
- device/vendor compatibility changes that weaken enforcement;
- build/release provenance failures.

## Development status

A feature being implemented, compiling, or demonstrated on a development device does not imply production security support. Device-specific security ceilings and vendor/firmware lifecycle limitations are tracked separately from platform functionality.
