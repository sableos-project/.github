# R8 consolidated build-plan review status

Status: **SECOND-EYE REVIEW COMPLETE — corrections incorporated; ready for final CI/merge.**

The experienced Android build-engineer review confirmed the A1/A2/B1/B2/B3 architecture and did not identify a blocking structural gap. The review did identify several implementation details worth adding, plus several claims that required AOSP verification before becoming normative.

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

No current Sable-owned application requires `android:sharedUserId`. Sable Start does not declare it. R8 therefore adopts **NO_NEW_SHARED_USER_ID** as the default product rule; any future exception would require an explicit architecture/security gate.

## Promotion condition

The coordinated R8 PRs may be promoted after their final CI/policy checks are green. Real A2/B1 Android evidence remains future execution work on `ai-g732`; documentation review does not pre-claim those runtime/build results.
