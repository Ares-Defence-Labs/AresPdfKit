# AresPdfKit

Flutter PDF SDK from Ares Defence Labs: **one SDK, two integration modes**.

**Preparation status:** local and unpublished. The documented API is implemented
in the current compiled candidate, with bounded feature coverage. This is not a
full Syncfusion parity or production-readiness claim. No package version is
announced as available by this repository.

| Integration | Entry point | Who owns the interface? |
| --- | --- | --- |
| Low-level PDF API | `AresPdfApi` | Your app supplies the UI and calls document operations. |
| Full viewer/editor | `AresPdfViewer` | Embed the supplied workspace; select tools, colors and host workflows. |

Both modes share the compiled PDF processing and offline licence enforcement.
Developers use one direct `ares_pdf` dependency and import
`package:ares_pdf/ares_pdf.dart`. Desktop and Web runtime packages are coordinated
transitive dependencies, not separate customer-facing PDF products.

This public repository contains **documentation and support material only**.
It is not an installable Git dependency. SDK implementation, native binaries,
Web runtime and the runnable example belong in the reviewed package distribution.
The metadata-only `pubspec.yaml` here is deliberately non-publishable.

## Start here

<img src="media/viewer.jpg" alt="AresScan+ PDF viewer" width="240"> <img src="media/drawing.jpg" alt="AresScan+ drawing workflow" width="240"> <img src="media/measurement.jpg" alt="AresScan+ measurement workflow" width="240">

Supplied Android AresScan+ workflow screenshots. Some shown app/legacy features
are still being migrated into the protected SDK. See the [seven-image visual guide](docs/visual-guide.md)
for accurate captions and current feature coverage.

- [Installation and first PDF](docs/getting-started.md)
- [SDK architecture and integration modes](docs/architecture.md)
- [Low-level API and ownership](docs/low-level-api.md)
- [Viewer/editor integration](docs/viewer-editor.md)
- [Themes, tools and controller state](docs/customization.md)
- [All nine host callbacks and I/O limits](docs/host-callbacks.md)
- [Complete public API reference: 89 types](docs/api-reference.md)
- [Pages, graphics, layout, fonts and images](docs/document-authoring.md)
- [Forms and annotations](docs/forms-annotations.md)
- [Attachments, metadata, bookmarks and search](docs/document-management.md)
- [Office conversion](docs/office-conversion.md)
- [Spreadsheet scalar math](docs/spreadsheet-math.md)
- [Certificate signatures and timestamps](docs/signatures.md)
- [Offline CRL/OCSP document revocation](docs/document-revocation.md)
- [Encryption, protected readers and optimization](docs/security.md)
- [Offline SDK licensing, trial and renewal](docs/licensing.md)
- [Platform setup and exact verification scope](docs/platforms.md)
- [Web hosting, Wasm and asset requirements](docs/web.md)
- [Example application](docs/example.md)
- [Errors and troubleshooting](docs/troubleshooting.md)
- [Migration and AresScan+ integration](docs/migration.md)
- [Feature coverage and remaining work](docs/feature-status.md)
- [Release/distribution model](docs/releases.md)

## Minimal low-level use

```dart
import 'package:ares_pdf/ares_pdf.dart';

Future<void> createPdf() async {
  final engine = await AresPdfApi.connect();
  try {
    final blank = await engine.createDocument();
    try {
      final edited = await blank.edit([
        const AresPdfText(page: 1, text: 'Hello PDF', x: 36, y: 72),
      ]);
      try {
        final bytes = await edited.exportBytes();
        // Save bytes with storage owned by your application.
      } finally {
        await edited.close();
      }
    } finally {
      await blank.close();
    }
  } finally {
    await engine.close();
  }
}
```

Connecting is normal SDK use and can start the real process/page trial. Do not
connect merely to preflight whether a customer might want a PDF feature later.

## Embed the editor

```dart
AresPdfViewer(
  bytes: pdfBytes,
  options: AresPdfViewerOptions(
    tools: const [
      AresPdfViewerTool.previous,
      AresPdfViewerTool.next,
      AresPdfViewerTool.zoomIn,
      AresPdfViewerTool.zoomOut,
      AresPdfViewerTool.undo,
      AresPdfViewerTool.redo,
      AresPdfViewerTool.exportPdf,
    ],
    onExport: savePdfBytes,
  ),
  onCreated: (controller) {
    // Retain this controller only while its widget/workspace is alive.
  },
  onError: (error) {
    // Present your app's error state.
  },
)
```

`pdfBytes` and `savePdfBytes` are application-owned values/functions. The full
[viewer guide](docs/viewer-editor.md) explains initialization, saving and cleanup.

## Platforms and licence

Android, iOS, macOS, Linux, Windows and Web have compiled members. Local candidate
checks executed Android emulator, iOS Simulator with SwiftPM/CocoaPods, macOS
programmatic checks, Linux debug/profile/release under containers/emulation and
Web in the in-app browser. **Windows runtime, macOS foreground UI, physical
devices and positive paid activation remain pending.** See [platforms](docs/platforms.md).

Production SDK use requires the separately approved commercial SDK terms and a
valid signed portal key. Missing, invalid or expired keys use the compiled trial
policy, including the exact popup:

> You are using a trial version of Ares Defence Labs PDF SDK

The trial is a genuine shared 60-second session. After expiry, advanced operations
are denied while supported reading remains available. Closing/reopening an
owner does not reset the trial. See [licensing](docs/licensing.md).

The repository's existing MIT licence covers its documentation/support material;
it does not grant rights to the separately distributed proprietary SDK binaries.
Final SDK terms and third-party redistribution review are publication gates.

[Developer portal](https://aresdefencelabs.com/development-sdks?sdk=pdf-library&view=overview)
· [Report an issue](https://github.com/Ares-Defence-Labs/AresPdfKit/issues)
· [Security reporting](SECURITY.md)
