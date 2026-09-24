# R8 consolidated build-plan review status

> **HISTORICAL R8 REVIEW RECORD — superseded current-state note (2026-09-24):** Pixel 7 / Panther R9 is physically accepted and frozen. K1/K2 multi-device artifact/deployment foundation is merged. Active work is keyboard-first common-product design plus Titan 2 / Titan 2 Elite research. Current authority: `docs/CURRENT_RELEASE_STATUS.md` and `docs/DEVELOPMENT_RELEASE_PLAN.md`.


Status: **MERGED / AUTHORITATIVE — second-eye review complete and corrections incorporated.**

The experienced Android build-engineer review confirmed the A1/A2/B1/B2/B3 architecture and did not identify a blocking structural gap. The resulting corrections were verified against current AOSP/Android behavior where needed, all coordinated documentation/tooling PRs passed their required CI/policy checks, and the consolidated R8 revision has been merged into the owning default branches.

## Adopted review additions

- inspect `module_bp_java_deps.json` when the target tree generates it;
- use Ninja edge inspection plus `-t inputs` where supported to prove the frozen A2 APK is a real graph input;
- inspect dexpreopt/uses-library configuration, including `optional_uses_libs` where the manifest actually declares optional shared libraries;
- verify native libraries are stored/aligned consistently with `extractNativeLibs` behavior;
- record native dynamic dependencies / unresolved-symbol inventory before device runtime testing;
- add source/dependency review for hard-coded page-size assumptions and require real 16 KiB runtime execution evidence;
- allow `snod` only as a bounded repackaging experiment after PRODUCT_OUT is known current, never as proof that dependencies were rebuilt;
- define fresh-output escalation as evidence-driven rather than reflexive.

## Corrected / rejected overstatements

The final R8 plan does **not** adopt the following claims as written:

- ordinary `/system/app` placement does not require a custom `file_contexts` entry by default; custom SELinux domain/seinfo/file-context work is required only when a documented capability actually needs it;
- `dex_preopt.copy_files` is not treated as a current `android_app_import` property;
- `ro.article.16kb.supported` is not used; current page-size evidence uses the actual AOSP/runtime properties plus `getconf PAGE_SIZE`/`PAGESIZE`;
- an ARM64 Android `.so` is not "host dlopen tested" on an unrelated x86 host; B1 uses ELF/dynamic-symbol inspection and B2/B3 perform real runtime loading;
- a simple `Cargo.lock` grep alone cannot prove absence of hard-coded 4096-byte assumptions; source/dependency review plus 16 KiB runtime testing is required;
- significant repo sync, SEPolicy edits or Soong changes are reasons to reassess incremental safety, not automatic authorization for `clean`/`clobber`.

## Application identity decision

No current Sable-owned application requires `android:sharedUserId`. Sable Start does not declare it. R8 therefore adopts **NO_NEW_SHARED_USER_ID** as the default product rule; any future exception requires an explicit architecture/security gate.

## Current execution boundary

The merged documentation/tooling defines the R8 plan; it does not pre-claim execution results that have not happened yet.

```text
A1 disposable qualification              architecture/current CI path
A2 trusted standalone app build          pending execution on ai-g732
B1 pre-image Android integration gate     pending execution on ai-g732
B2 Panther development image/runtime      not yet authorized by plan merge alone
B3 Titan 2 portability image/runtime      follows Panther acceptance
production signing                        deferred until dual-target development qualification
```

The next evidence-producing work is therefore A2/B1 on the migrated, sealed `ai-g732` environment, not another broad image build.
