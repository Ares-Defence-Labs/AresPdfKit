# One SDK, two modes

```text
Flutter application
  └─ package:ares_pdf/ares_pdf.dart
       ├─ AresPdfApi → AresPdfEngine → owned document snapshots
       └─ AresPdfViewer → AresPdfViewerController → protected workspace
              ↓
       compiled platform transports + shared compiled PDF Core
              ↓
       PDF parsing, editing, rendering, crypto and offline licensing
```

## Low-level mode

Use `AresPdfApi.connect()` when your app supplies the interface or runs document
operations without showing a PDF. The engine owns document handles. Mutation
methods return new snapshots; input documents stay unchanged. You can use this
mode alongside a viewer, but do not exchange owner-local font/image IDs between
independent engines/workspaces. Bytes are the cross-owner interchange format.

## Viewer/editor mode

`AresPdfViewer` embeds the supplied workspace. Its controller changes navigation,
configuration and the current document through typed operations. A toolbar is
optional; `tools: []` hides it without granting document permissions or disabling
licence enforcement. Typed controller edits work on the existing compiled document,
with native revision/history/resource checks.

The application supplies file selection, storage and connected signing through
typed callbacks. A callback provides I/O; it is not an alternative PDF engine or
an entitlement decision. See [host callbacks](host-callbacks.md).

## Package boundaries

Customers depend directly on `ares_pdf`. The current protected architecture also
has coordinated transitive members `ares_pdf_protected_desktop` and
`ares_pdf_protected_web`. They retain the native registrar and browser asset
namespaces. Pure shared DTOs may have `PortablePdf...` compatibility names;
the documented `AresPdf...` aliases refer to the same Dart type identity.

PDF implementation and licence authority remain compiled. The public Dart layer
contains typed DTOs, widget lifecycle and transports; it is not entirely opaque
source. Native code/binaries and WebAssembly can be inspected or reverse-engineered
by determined attackers; compiled delivery is not a guarantee against all tampering.

## Capability, permission and licence are different

`capabilities()` reports immutable actual compiled support. A newer Dart facade
cannot add a missing binary feature. PDF document permissions and the SDK licence
are checked independently by compiled code. No public callback, tool selection,
theme or capability map may turn an unlicensed session into a licensed one.

SDK licence checks run locally. Optional **document certificate** CRL/OCSP checks
consume public evidence supplied by your application; they do not contact the
licensing backend or revoke SDK customer keys. See [document revocation](document-revocation.md).

## Scope

Both modes target six platforms, with the verification distinctions listed in
[platforms](platforms.md). The current feature set is bounded. A method, native
build or host callback is not evidence of complete Office/PDF standards parity.
Read [feature status](feature-status.md) before selecting workflows.
