# Titan 2 N1B findings and decision gate

Status: **decision gate before additional Titan 2 builds or flashes**
Date: 2026-09-30
Repos in scope:

- `sableos-project/.github`
- `aimindseye/sableos`
- `aimindseye/unihertz-titan2`

```text
TITAN2_N1B_STATUS=PAUSED_FOR_DECISION_GATE
NEW_BUILD_AUTHORIZED=NO
NEW_FLASH_AUTHORIZED=NO
PR_MERGE_CLAIM=NO
PUBLIC_RELEASE_CLAIM=NO
TRIAL_AND_ERROR_CONTINUATION=STOPPED
BENCHMARK_FIRST_DECISION_GATE=REQUIRED
```

## Executive summary

Titan 2 N1B work proved that SableOS/Restless-derived GSI images can be built,
packaged, resized and flashed to Titan 2 `system_a` while preserving userdata and
stock boot/vendor/vbmeta partitions. The blocker is no longer basic image
construction. The blocker is runtime compatibility with Titan 2's MediaTek
vendor/modem stack.

The current evidence shows a repeated pattern:

```text
LTE_OR_LTE_ANCHOR_RETURNS=YES
NR_NSA_DISPLAY_OR_ANCHOR_SIGNAL_APPEARS=YES
WWAN_DATA_SESSION_STABILITY=FAIL
DATA_CALL_FAIL_CAUSE=ERROR_UNSPECIFIED_0xffff
POST_DROP_STATE=OUT_OF_SERVICE_OR_EMERGENCY_OR_UNKNOWN
NETWORK_REJECT_CAUSES_SEEN=11,13,15,114
```

N1B4 corrected one real defect, the MTK IMS overlay path that pointed AOSP IMS
resolution at a stripped/nonexistent `com.mediatek.ims`. It did not make Titan 2
cellular stable enough for a release claim.

## What was proven

### Build and packaging

```text
SABLE_TITAN2_GSI_BUILD=PROVEN
TARGET_FILES_BUILD=PROVEN
SYSTEM_IMG_BUILD=PROVEN
TARGET_FILES_VERIFICATION=PROVEN
ARTIFACT_SHA_SEALING=PROVEN
```

N1B image work established repeatable source-side product deltas and target-files
verification for Titan 2 GSI artifacts.

### Flashing model

```text
FASTBOOTD_USERSpace_FLASH=PROVEN
ACTIVE_SLOT=a
SYSTEM_A_RESIZE_AND_FLASH=PROVEN
USERDATA_PRESERVE=PROVEN
BOOT_VENDOR_VBMETA_FLASHED=NO
FULL_SUPER_FLASHED=NO
```

The proven Sable path so far is **system partition only**, not a full-super or
stock-super rebuild path.

### N1B3/N1B4 cellular changes

N1B3 introduced explicit radio compatibility markers and held the line against
importing stock MTK telephony APK/JAR/SO payloads.

N1B4 removed the static MTK IMS overlay path from the produced target-files image:

```text
TREBLE_MTK_IMS_OVERLAY_REMOVED=YES
COM_MEDIATEK_IMS_BINDING_REMOVED=YES
STOCK_MTK_TELEPHONY_PAYLOAD_IMPORTED=NO
CELLULAR_SCOPE=LTE_DATA_ONLY_OR_RESEARCH
```

The N1B4 change is useful as a prerequisite, but it is **not sufficient** for
stable cellular.

## Runtime findings

### MTK IMS overlay removal was correct but insufficient

Before N1B4, AOSP IMS resolution could bind toward `com.mediatek.ims` even though
that payload was intentionally absent. Removing the MTK IMS overlay eliminated
that specific failure path.

After N1B4, Titan 2 still shows a short cellular recovery window followed by WWAN
packet-service collapse:

```text
LTE_HOME_WINDOW=BRIEF
NR_NSA_OR_ANCHOR_DISPLAY=BRIEF
WWAN_DATA_CALL=CONNECTS_OR_ATTEMPTS
WWAN_PS=NOT_REG_SEARCHING_OR_EMERGENCY_AFTER_DROP
DATA_ALLOWED=false
DISALLOW_REASONS=SERVICE_OPTION_NOT_SUPPORTED,NOT_IN_SERVICE
```

### IWLAN/QNS-only hypothesis did not hold

A runtime experiment temporarily disabled `com.google.android.iwlan`. Cellular
still dropped. IWLAN/QNS may add noise and should remain under review, but the
IWLAN package alone is not the demonstrated root cause.

```text
IWLAN_DISABLE_STABILIZED_CELLULAR=NO
QNS_ONLY_ROOT_CAUSE=UNPROVEN
```

### APN-only hypothesis did not hold

The attempted switch from the current US Mobile `pwg` APN row to a different
numeric row did not stick. Current evidence does not justify shipping an APN
overlay as N1B5.

```text
APN_310260_EXPERIMENT=NOT_ACTUALLY_APPLIED
APN_OVERLAY_READY=NO
```

### LTE-only test path was inconclusive

Multiple `cmd phone set-allowed-network-types-for-users` attempts failed with
`No valid NETWORK_TYPES_BITMASK`; no ADB network-mask LTE-only test actually ran.
A manual UI LTE-only direction still produced a short LTE/NR-NSA display window
and then disconnected, so it does not currently prove that disabling NR solves
the issue.

```text
ADB_ALLOWED_NETWORK_MASK_MUTATION=FAILED_TO_APPLY
UI_LTE_ONLY_RESULT=DISCONNECTED
NR_DISABLE_FIX_PROVEN=NO
```

## Why this is a platform decision, not another patch loop

The Titan 2 behaves like a device-specific MediaTek port, not like a Pixel-like
AOSP target. The current Sable approach is a GSI/system-image approach that
preserves the stock vendor side. That can boot, but cellular stability appears to
require a deeper comparison against stock and existing Titan 2 GSI work.

Relevant outside baselines:

- `agreenbhm/Unihertz-Titan-2-LineageOS`
- the downloaded local file `Titan2-LineageOS-23.0-20251027-GAPPS-EXT4-GSI-v0.0.2.img.gz`
- `PeterGSI/android_device_peter_gsi`
- stock Titan 2 firmware/runtime behavior

The LineageOS Titan 2 reference appears to use Titan-specific GSI customizations
and a stock-super/dynamic-partition oriented build flow rather than a pure
system-only replacement. That difference must be studied before the next Sable
build or flash.

## Options under review

### Option A — Pause Titan 2 as an immediate SableOS release target

Move main SableOS development back to the Pixel/R9 path and keep Titan 2 as a
research target until stock/Lineage/PeterGSI baselines prove a safe path.

### Option B — Use LineageOS 23 Titan 2 as reference baseline

Inspect and benchmark the downloaded LineageOS 23 Titan 2 image and the
`agreenbhm` customization repo. If cellular is stable there, compare what it
preserves or changes relative to Sable N1B.

### Option C — Shift Sable Titan 2 to stock-super plus Sable-system architecture

Adopt a fuller dynamic-partition/super-image strategy if benchmarking shows that
Titan 2 needs stock `vendor`, `system_ext`, `vendor_dlkm`, `odm_dlkm` and runtime
hooks packaged together with the GSI system image.

### Option D — Stock firmware plus privacy hardening/debloat

If all GSI paths remain unstable and daily-driver cellular matters, keep stock
firmware as the base and harden/debloat rather than pursuing a near-term full
SableOS replacement.

## Benchmark-first decision gate

```text
TITAN2_DECISION_GATE_R1=REQUIRED
NO_NEW_SABLE_BUILD_UNTIL_GATE=YES
NO_FLASH_UNTIL_GATE=YES
NO_PR_MERGE_CLAIM_UNTIL_GATE=YES
```

The decision gate should answer:

1. Does stock Titan 2 keep cellular stable on the same SIM/location?
2. Does the downloaded LineageOS 23 Titan 2 image keep cellular stable?
3. Is the Lineage image a `system.img` or `super.img` style artifact?
4. What Titan-specific runtime hooks are present in Lineage/PeterGSI?
5. Does Lineage/PeterGSI disable, preserve or replace MTK IMS/EPDG/IWLAN/QNS paths?
6. Does the stable path require full-super packaging rather than `system_a` only?
7. Which option, A/B/C/D, should SableOS choose after evidence comparison?

## Current decision

Do not continue N1B4/N1B5 cellular trial-and-error. Record evidence, benchmark
external Titan 2 GSI work, then choose the architecture path deliberately.
