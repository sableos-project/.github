# R8 consolidated build-plan second-eye review

Status: **review brief for experienced Android build engineering feedback.**

The normative details are in `DEVELOPMENT_RELEASE_PLAN.md`, `CI_TRUST_ARCHITECTURE.md`, `SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md` and the owning repository documents.

## Proposed pipeline

```text
A1 — disposable qualification
GitHub-hosted Rust/Kotlin/Gradle/static/security CI
        |
        v
A2 — trusted standalone app build on ai-g732
pinned toolchains, APK/JNI provenance, 16 KiB compatibility
        |
        v
exact trusted R8 application freeze
        |
        v
B1 — pre-image Android/Soong integration on ai-g732
android_app_import candidate, graph/query, signing/JNI/dexpreopt/
uses-library, product-selection and PRODUCT_OUT proof
        |
        v
B2 — Panther development image + runtime/regression acceptance
        |
        v
B3 — Titan 2 development image + portability acceptance
same frozen common app artifacts where compatible
        |
        v
LATER — production signing/release workstream
ThinkPad P50 is only a future signing-host candidate
```

Production AVB/OTA/application signing is intentionally deferred until development builds/runtime are satisfactory on both Panther and Titan 2. OptiPlex is removed from the plan and no active `sable-signer-01` exists yet.

## Application/product principles

- AOSP is an integration environment, not the everyday app compiler.
- GitHub CI artifacts are qualification evidence, not automatically trusted product binaries.
- `ai-g732` produces the trusted standalone application artifacts and development images.
- `android_app_import` is preferred but exact Android 17/GrapheneOS behavior is measured before normalization.
- Common imported modules and common `PRODUCT_PACKAGES` selection belong in `vendor_sable`.
- Device repos contain only genuine target exceptions/adapters.
- Module build, product selection, PRODUCT_OUT, target-files, image and runtime remain separate claims.
- No automatic clean/clobber/delete on failure; fix/classify and rerun the narrowest valid target.
- OUT_DIR is isolated per target/materially different variant.
- Outer APK hashes may change through legitimate Soong/signing processing; DEX/JNI inner-content identities are tracked separately.
- Native R8 libraries require verified 16 KiB compatibility.
- Image inspection detects filesystem type before choosing ext4/EROFS tooling.

## R8 app tranche

- R8-A shared design: Follow system / Light / Dark / bounded accent / reset.
- R8-B Calculator + Convert.
- R8-C Games: Sudoku / Minesweeper / 2048.
- R8-D Reader publication path: Vaachak Mobile / Readium.
- R8-D2 Reader text/accessibility: TXT / share/process-text / TTS / OCR.
- R8-E Media: local Music + Internet Radio.

## Titan 2 portability target

| Dimension | Panther | Titan 2 |
| --- | --- | --- |
| ABI | arm64-v8a | arm64-v8a |
| Rust target | aarch64-linux-android | aarch64-linux-android |
| 16 KiB compatibility | required | required |
| common app artifacts | frozen/common | same where compatible |
| common product integration | vendor_sable | vendor_sable |
| OUT_DIR | isolated | isolated |
| runtime page size | measured | measured |
| physical keyboard | baseline | explicit gate |
| square display | baseline | explicit gate |
| Reader OCR/TTS | capability gate | capability gate |
| Media3/audio | capability gate | capability gate |
| production signing | deferred | deferred |

Secondary-display/program-key/FM features are not common R8 requirements unless separately approved.

## Second-eye questions

1. Is A1 -> A2 -> B1 -> B2 -> B3 the right boundary for Android 17/GrapheneOS development?
2. Are there any reasons the trusted standalone app build should happen inside the Android tree rather than adjacent to it on the same trusted host?
3. For `android_app_import`, which additional Soong intermediates/config files/graph queries should B1 capture?
4. Is whole-APK plus extracted DEX/JNI hashing sufficient to distinguish legitimate container/signing changes from code drift?
5. Beyond ELF `PT_LOAD` alignment and APK 16 KiB ZIP alignment, what native-page-size checks should be mandatory?
6. Which narrow targets give the strongest product-selection/PRODUCT_OUT confidence before target-files/image work?
7. Which dexpreopt, uses-library, certificate, JNI-layout or partition edge cases are most likely to invalidate this plan?
8. For Panther/Titan portability, what state besides OUT_DIR must always remain target-isolated?
9. Which failure classes should force a fresh OUT instead of incremental continuation?
10. Is production-signing deferral until dual-target development qualification a sound sequencing decision, or is there any signing-related behavior that should still be exercised earlier with development keys?

The objective is to make full image builds final integration proofs rather than the first place ordinary app/product-wiring problems are discovered.
