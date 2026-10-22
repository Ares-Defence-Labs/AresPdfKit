# Example application

The reviewed SDK distribution includes an `example/` Flutter application that
uses only `package:ares_pdf/ares_pdf.dart`. It demonstrates low-level creation,
text editing/export to app-owned memory, and the full viewer in the same host.

The documentation-only AresPdfKit repository intentionally does not include SDK
implementation or runnable example source. Use the supplied local SDK's example;
a future published version can expose it through pub.dev's Example tab.

```sh
cd example
flutter pub get
flutter run
```

To supply your own portal key, use the `ARES_PDF_LICENCE` build define. Never
commit a real key. Without a valid key, the example exercises the actual shared
trial; do not treat a rebuild/owner recreation as an expiry reset.

Select **Create PDF (API)** to generate a document and open it in the editor.
The example's export callback retains an owned byte copy in memory and reports
its size. It does not pretend to implement native file saving. Replace that
callback with your app's storage adapter. Office files, fonts and signing
identities must be supplied by the host integration; none is fetched implicitly.

Run on each target you intend to support. A successful example compile is distinct
from native runtime, saved-output and positive-key acceptance. See [platforms](platforms.md).
