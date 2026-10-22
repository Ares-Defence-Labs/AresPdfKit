# Installation and first PDF

The documented package is being prepared locally. There is no announced public
release to install yet. Do not add this documentation repository as a Git dependency.

## Local reviewed SDK

Extract the supplied SDK keeping `ares_pdf` and both `components` directories
together. Point your application at the facade:

```yaml
dependencies:
  flutter:
    sdk: flutter
  ares_pdf:
    path: ../reviewed-sdk/ares_pdf
```

Use the exact Flutter/Dart toolchain and platform prerequisites in
[platforms](platforms.md). Run `flutter pub get`, then build a fresh host app;
hot reload cannot install native plugin changes. No native library path or
application identifier override is accepted by the public factory.

The planned hosted layout uses one direct dependency, with version-matched
desktop and Web transitive members. Hosted dependency examples will be activated
only when those package versions are actually available.

## First operation

Import `package:ares_pdf/ares_pdf.dart`. Follow the complete create/edit/export
example in [README](../README.md#minimal-low-level-use). Each returned document
is independently owned. Await its close in `finally`; close the engine last.
Use [the package example](example.md) to exercise both modes in the same app.

## Licence before real customer use

```dart
final engine = await AresPdfApi.connect();
try {
  const key = String.fromEnvironment('ARES_PDF_LICENCE');
  if (key.isNotEmpty) {
    final accepted = await engine.activate(key);
    // false means trial policy applies; it is not a licence override.
  }
  final state = await engine.licenceState();
  // state.licensed and state.trialRemaining are native snapshots.
} finally {
  await engine.close();
}
```

This snippet shows activation/state only; in a real app retain the owner for its
document work. Use the same initialized runtime for the viewer. Do not use a
temporary owner as a way to reset a trial. Production positive-key acceptance
is still a separate release gate; this documentation does not claim it passed.

Supply the full portal-issued signed string, not its account record ID. Do not
commit a real customer key to public source. Your deployed app must contain a
renewed key before the current key expires. See [licensing](licensing.md).

## Host responsibilities

Your app loads/saves bytes, chooses files, obtains fonts, owns destinations and
handles errors. The SDK parses/manipulates PDFs, converts supported Office input,
renders pages and checks its licence in compiled code. No file-picker, storage
provider or billing backend is required merely to create a PDF from bytes.

For Web, serve the complete compiled runtime on the same origin with the required
headers. See [Web setup](web.md); a native-only smoke test does not test browser
Wasm delivery.
