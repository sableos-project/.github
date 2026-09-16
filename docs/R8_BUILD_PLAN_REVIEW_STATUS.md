# R8 consolidated build-plan review status

Status: **DRAFT — awaiting experienced Android build-engineer second-eye review.**

This branch intentionally stages the A1/A2/B1/B2/B3 dual-target build architecture without merging it to `main` yet.

Review focus:

- trusted standalone app-build boundary on `ai-g732`;
- `android_app_import` / Soong pre-image proof;
- 16 KiB native compatibility gate;
- Panther/Titan 2 common-artifact portability;
- isolated OUT_DIR/cache/failure policy;
- production-signing deferral and future ThinkPad signing-host candidacy.

The draft can be promoted after the review identifies no blocking architecture gap or after any resulting corrections are incorporated.
