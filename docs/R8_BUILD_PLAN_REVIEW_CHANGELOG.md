# R8 consolidated build-plan changelog

This draft revision changes the prior R8 plan in these material ways:

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
