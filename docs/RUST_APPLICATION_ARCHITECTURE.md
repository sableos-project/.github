# SableOS Rust / Android application architecture

Status: **current normative ownership guidance — 2026-09-24**

SableOS may use Rust where it materially improves owned logic, memory safety,
testability or deterministic domain behavior. Android lifecycle, UI, platform
services and OS integration remain in the language/framework layer that best
matches Android ownership.

This is not a “rewrite everything in Rust” policy.

## Core split

A typical Sable application may use:

```text
Rust
    deterministic domain logic
    parsing/state machines where appropriate
    pure transforms
    algorithms that benefit from strong memory-safety/testability

Kotlin / Android
    lifecycle
    Compose/UI
    intents/roles
    package/profile APIs
    permissions
    MediaSession/Camera2/etc.
    notifications
    accessibility
    system service integration
```

JNI/FFI is a boundary to minimize and test, not an architectural goal.

## Current HOME boundary

The approved launcher architecture is:

```text
org.sableos.launcher / SableLauncher
    user-facing HOME and Sable launcher semantics

Launcher3QuickStep
    Recents / Overview / task / gesture substrate
```

Historical SableStart source is presentation/reference history, not current HOME
authority.

Rust launcher-domain logic, if used, must not recreate Android task/role/security
services.

## Application guidance

### Calculator / converters

Rust is appropriate for deterministic arithmetic/conversion logic where it
improves correctness/testability. Android owns presentation, lifecycle,
accessibility and input.

### Games

Rust may own deterministic game rules/state. Android owns UI, lifecycle,
accessibility and keyboard/touch input. Do not introduce a Lua runtime merely to
port legacy architecture.

### Media

Rust may own parsing/catalog/state logic where useful. Android owns media
decoding, MediaSession, audio focus/routing, storage and network integration.

### Reader / Text Reader

Do not replace mature Readium/publication rendering simply to increase Rust
usage.

Sable Reader and Sable Text Reader are separate accepted products. Their core
capability boundaries remain traceable to their qualified upstream/source
provenance.

### Camera

Camera2/vendor HAL interaction remains Android/platform-facing. Rust may be used
for bounded pure capability/state logic if justified, but not as a replacement
for Android camera lifecycle/session ownership.

### Keyboard / IME

Android owns IME lifecycle, InputMethodService behavior, editor interaction and
hardware-input APIs. Pure layout/compose/transformation logic may use Rust only
when the FFI/testing tradeoff is favorable.

## Keyboard-first architecture

Keyboard-first product behavior is common application architecture, not a
Titan-specific source fork.

Rust/Kotlin ownership does not change the requirements for deterministic focus,
type-to-search, shortcuts, stable focus restoration, square layouts and
accessibility.

Device-specific scan codes, keylayout/keycharacter maps, backlight and
pointer/touch-surface quirks belong in the device/platform adapter rather than
common Rust domain code.

## Native compatibility

Any APK containing native libraries must preserve exact native provenance and
pass the target platform's native/page-size requirements.

The accepted Panther reference demonstrated the importance of separating:

```text
source build PASS
APK/JNI identity
product integration
image membership
runtime JNI execution
```

Future targets repeat the relevant proof independently.

## Security

Rust reduces some memory-safety risk but does not replace threat modeling,
permission minimization, parser limits, fuzzing where appropriate, dependency
review, update ownership or Android sandboxing.

Do not add a native layer where Kotlin/Android code is already simpler and safer
for the owned behavior.

## Current device state

Panther is REFERENCE_FROZEN. Titan 2 / Titan 2 Elite are keyboard-first
portability targets. Q27 remains research.

No device role justifies a common-application source fork.
