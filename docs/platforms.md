# Platform setup and verification scope

The current candidate requires Dart `^3.12.0` and Flutter `>=3.47.0`. These are
actual candidate constraints, not compatibility promises for older Flutter.

| Platform | Prepared member | Local verification | Still pending |
| --- | --- | --- | --- |
| Android | Flutter plugin with compiled Core/viewer | ARM64 emulator, actual app-owned accessibility controls and API/viewer workflows | Physical devices and broader device/ABI acceptance |
| iOS | Compiled frameworks/native viewer | Simulator; SwiftPM and CocoaPods consumers; XCTest controls | Physical devices and general OS/device coverage |
| macOS | Universal compiled frameworks/native plugin | ARM64 programmatic API/viewer plus native offscreen component checks | Foreground UI/gestures/Export at user request; physical Intel acceptance |
| Linux | Compiled native plugin/Core | Debug/profile/release Flutter x64 in local Docker/emulation; native ARM64/x64 component checks | User hardware/desktop/window-manager/GPU acceptance; registered ARM64 Flutter delivery |
| Windows | x64 compiled native members and public Dart test kit | Cross-compilation/import audit and Dart bundle preflight | Windows native app/runtime, dialogs, gestures, accessibility and hardware |
| Web | Compiled Wasm viewer/Core | Actual Wasm browser consumer in the in-app browser | Wider browser/device/GPU matrix |

Eight canonical runs cover Android, both iOS dependency managers, macOS
programmatically, Linux in all three modes and Web. Each includes genuine expiry
and retained reader checks. This is not eight different supported operating
systems or a complete hardware matrix.

## Native hosts

Use a fresh Flutter host after changing binary/plugin dependencies. Do not copy
generated plugin registrations into an unrelated app manually. Preserve the SDK's
framework symlinks and signatures when extracting on macOS; use an extractor that
preserves links. Windows extraction can materialize symlinks as ordinary files:
re-extract the original archive on macOS before using Apple members.

iOS test consumers use deployment target 16.0. macOS consumers use 12.0. The
current Android member declares minSdk 23, compileSdk 36 and Java/Kotlin JVM 17.
These are manifest constraints, not physical-device qualification. Native
Android/desktop toolchain constraints must match the supplied member manifests;
do not lower minimums or add an unsupported ABI based only on a successful Dart
compile. The final package metadata and supported OS-version matrix need final
release acceptance, including Android minimum/toolchain review.

SwiftPM and CocoaPods are separate integration routes with separate local
receipts. Do not mix their generated artifacts or conclude one route passed from
the other's build. Windows owner testing should preserve output PDFs, logs and
the runtime receipt; compile success is insufficient.

## Web

Follow [Web setup](web.md). Native bundles intentionally exclude Web runtime
assets. Do not move runtime files independently or rename asset namespaces.

## Acceptance limits

Positive paid activation, physical-device verification and broader UI coverage
remain separate from the passing trial-mode tests. Cleanup proves tracked
ownership drains; it does not prove immediate GPU allocation destruction.
See [feature status](feature-status.md) for the remaining functionality gaps.
