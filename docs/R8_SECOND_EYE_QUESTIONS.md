# R8 second-eye questions

1. Is A1 -> A2 -> B1 -> B2 -> B3 the right boundary for Android 17/GrapheneOS development?
2. Should the trusted standalone app build remain outside the Android tree on the same trusted host?
3. Which Soong intermediates/configs/graph queries should B1 capture for `android_app_import`?
4. Is whole-APK plus extracted DEX/JNI hashing sufficient to distinguish legitimate container/signing changes from code drift?
5. Beyond ELF `PT_LOAD` and APK 16 KiB alignment checks, what page-size validation is missing?
6. Which narrow targets give the strongest product-selection/PRODUCT_OUT confidence before broad image work?
7. Which dexpreopt/uses-library/certificate/JNI-layout/partition edge cases should be explicit gates?
8. What state besides OUT_DIR must remain target-isolated for Panther vs Titan 2?
9. Which failures should force fresh output rather than incremental continuation?
10. Is production-signing deferral until dual-target development qualification sound, and what signing-related behavior should still be exercised earlier with development keys?
