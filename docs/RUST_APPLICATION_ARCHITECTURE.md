# Rust application architecture

Status: **current normative guidance — 2026-09-24**

SableOS uses Rust where it improves memory-safety, deterministic domain logic or
testability without forcing Android lifecycle/UI/platform ownership into an
unnatural language boundary.

## Ownership rule

Prefer:

```text
Rust
    pure domain logic
    parsers/codecs where owned and justified
    deterministic state machines
    algorithms
    portable data transforms

Kotlin / Android
    Activity/Service lifecycle
    Compose/View UI
    permissions and roles
    PackageManager/LauncherApps
    MediaSession/Camera2/Telecom/etc.
    accessibility
    Android storage/content-provider APIs
```

Do not add Rust simply to increase Rust percentage.

## FFI boundary

JNI/FFI surfaces should be narrow, typed and testable.

Avoid:
- Android framework objects crossing deeply into Rust;
- opaque native handles without lifecycle ownership;
- unnecessary global mutable native state;
- native libraries for trivial UI/controller logic.

Every native artifact requires exact ABI/JNI identity and page-size
compatibility evidence.

## Common app rule

Rust/Kotlin ownership is common-product architecture. It does not fork by
Panther/Titan/Q27.

Keyboard-first adaptation belongs primarily in Android UI/input layers unless a
portable domain primitive genuinely belongs in shared Rust.

## Current launcher boundary

SableLauncher is the product HOME. Launcher3QuickStep remains Recents/task
substrate.

Launcher package/profile/focus/Compose behavior is Android-facing. A small Rust
domain core may be appropriate only for isolated deterministic state, not for
replacing LauncherApps/PackageManager/task APIs.

## Application examples

### Calculator / conversion

Rust is suitable for deterministic arithmetic/conversion engines and tests.
Kotlin/Compose owns Android presentation/input/accessibility.

### Games

Rust may own deterministic Sudoku/Minesweeper/2048 rules/state. Android owns
rendering, focus/touch/keyboard input, lifecycle and accessibility.

### Media

Android owns Media3, MediaSession, audio focus/routing/storage/network lifecycle.
Rust may own portable parsing/state where it is clearly beneficial.

### Reader / Text Reader

Do not replace proven Readium/Android reading stacks merely to introduce Rust.
Portable text parsing/stream processing may be Rust where independent and
well-tested.

Reader and Text Reader are separate current products.

### Camera

Camera2/vendor HAL/session ownership remains Android-facing. Rust is not a reason
to wrap the entire camera stack. Portable image/domain processing may use Rust
when it has a clear safety/testability benefit.

### Mail

Android/Kotlin retains account/service/UI/platform integration. Reused upstream
mail protocol/security code remains governed by its proven architecture rather
than being rewritten for language consistency.

## Security

Rust reduces classes of memory-safety bugs; it does not prove parser correctness,
crypto correctness, privilege safety or update lifecycle.

Use fuzz/property/unit tests where the owned Rust boundary materially benefits.

## Build evidence

For native code preserve:

- exact source/dependency lock state;
- toolchain/target;
- ABI list;
- JNI exports/imports where relevant;
- ELF and APK alignment/page-size compatibility;
- runtime execution evidence.

## Architecture-change rule

Do not replace an inherited/reused application or platform component merely to
obtain more Rust. Replacement requires the normal product/security/update
justification.
