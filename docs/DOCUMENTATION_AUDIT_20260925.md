# SableOS public documentation audit — 2026-09-25

## Why this audit exists

The 2026-09-25 cleanup merged the final Panther R9 Sable Hub V1 closure and the
keyboard-first design V1 documentation, closed stale broad R8/R9 issues and left
Titan/keyboard-first and specific polish issues open intentionally.

This audit records how the public `sableos-project` repositories should now be
read.

## Current private integration authority

```text
PRIVATE_MAIN_HEAD=2624e1af
R10_KEYBOARD_FIRST_DESIGN_V1=MERGED_PR_108
R10_KEYBOARD_FIRST_DESIGN_SOURCE=20da2daa
R9_PANTHER_HUB_V1_CLOSURE=MERGED_PR_110
R9_PANTHER_HUB_V1_MERGE=f175b00f
R9_PANTHER_ACCEPTED_SOURCE=edf62e5bb08372a1395841d6cc5d78d3148a7695
R9_PANTHER_TARGET_FILES_SHA256=a0b359613c4f30e9a834fba212e0b044a97d63ed0537c59471c31b99b627d285
R9_PANTHER_PHYSICAL_ACCEPTANCE=PASS_WITH_PRESERVED_PLAY_STATE
```

The earlier `6f1d6d2f` / `08ef...` Panther reference remains historical evidence
but is no longer the current organization-level Panther image authority.

## Public repositories reviewed

```text
sableos-project/.github
sableos-project/platform_manifest
sableos-project/platform_sable
sableos-project/vendor_sable
sableos-project/device_sable_panther
sableos-project/build
sableos-project/packages_apps_SableStart
```

## Reconciled status

| Area | Current status |
| --- | --- |
| Panther | Frozen touch-first R9 Hub V1 reference |
| Titan 2 | Active keyboard-first N0 portability target |
| Titan 2 Elite | Independent N0 target after its own hardware baseline |
| Q27 | Research/future candidate only |
| Build/deploy | `build/sable.sh <device> <release> <function>` is canonical |
| Artifact identity | Device + release + source + artifact hashes; serial is deployment-only |
| Open issues | Kept open until Titan 2 install closure |
| Production signing | Deferred |

## Repositories and required reading

- `.github/profile/README.md` and `.github/docs/CURRENT_RELEASE_STATUS.md` are
  the public organization status entry points.
- `platform_manifest` owns source/artifact composition and must distinguish
  Panther target-files from future Titan GSI/system artifacts.
- `platform_sable` owns common Sable semantic/design/portability contracts.
- `vendor_sable` owns common product composition, not device-specific forks.
- `device_sable_panther` owns the frozen Panther adapter/evidence role.
- `build` owns the multi-device operator, artifact and deployment contracts.
- `packages_apps_SableStart` is historical/common presentation reference until a
  clearly owned public SableLauncher/application repository supersedes it.

## Open issue policy

The remaining private integration issues are intentionally open until Titan 2
SableOS install work proves or supersedes them. Do not close them merely because
Panther is frozen.

Kept-open categories:

- Titan 2 / Titan 2 Elite / Q27 keyboard-first qualification;
- Sable Tools;
- SystemUI/appearance/Reader propagation;
- Media large-library performance;
- duplicate visible Messages review;
- production icons and launch timing;
- first-run crash evidence capture;
- All Apps granted-permission summaries;
- common SableLauncher focus/shortcuts/square layout;
- Sable Camera;
- Sable Keyboard / physical-keyboard adapter boundary.

## Documentation rule

If a historical R8/R9 planning document conflicts with the current status files,
treat it as historical evidence only. The current authority chain is:

```text
.github/docs/CURRENT_RELEASE_STATUS.md
  -> private integration CURRENT_STATUS.md while the private repo remains release authority
  -> accepted ADR-0010 / ADR-0011
  -> public common architecture/build/product docs
  -> historical evidence records
```
