# R8 consolidated build-plan changelog

This revision changes the prior R8 plan in these material ways:

1. Splits standalone app work into **A1 disposable qualification** and **A2 trusted standalone build**.
2. Makes `ai-g732` the trusted application/product development builder after migration sealing.
3. Adds **B1 pre-image Soong/product integration** before any broad image build.
4. Keeps Panther as **B2 primary development/runtime target**.
5. Adds Titan 2 as **B3 active portability target** using the same common frozen app artifacts where compatible.
6. Makes **16 KiB native compatibility** a required R8 gate.
7. Tracks whole-APK and inner DEX/JNI identities separately.
8. Requires isolated OUT_DIR per target/materially different variant.
9. Makes common imported modules/product app selection a `vendor_sable` responsibility.
10. Removes OptiPlex from the signing architecture.
11. Treats ThinkPad P50 as a **future signing-host candidate only**, not an active signer.
12. Defers production APK/AVB/OTA signing until Panther and Titan 2 development qualification is satisfactory.
13. Adds read-only artifact-audit tooling and a host self-test in `sableos-project/build`.
14. Adds `module_bp_java_deps.json` and Ninja `-t inputs` as optional B1 dependency evidence when present/supported.
15. Makes SELinux review conditional on actual privilege/domain requirements rather than requiring custom `file_contexts` for every system APK.
16. Adds `optional_uses_libs`, JNI compression/alignment, DT_NEEDED/undefined-symbol inventory and 16 KiB runtime execution to B1/B2/B3 evidence.
17. Treats `snod` only as an optional no-dependencies repackaging experiment; it is not dependency-closure proof, especially with dexpreopt enabled.
18. Uses actual AOSP/runtime page-size evidence (`ro.product.page_size`, `ro.product.cpu.pagesize.max`, `ro.product.build.16k_page.enabled` where applicable, and runtime `getconf`) rather than invented properties.
19. Adopts **NO_NEW_SHARED_USER_ID** for R8 applications; no current Sable app, including Sable Start, requires `android:sharedUserId`.
20. Makes fresh-output escalation evidence-driven: major Soong/toolchain/source-policy changes trigger reassessment, not automatic `clean`/`clobber`.
