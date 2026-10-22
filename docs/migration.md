# Migration and AresScan+ integration

The current public integration is bytes-based:

```dart
import 'package:ares_pdf/ares_pdf.dart';
```

It is different from the older internal `ares_pdf_engine` filesystem API and
GlobalKey/state-based viewer. Do not mix their method names or assume a legacy
app callback exists in the canonical SDK.

| Legacy pattern | Canonical integration |
| --- | --- |
| Local input path | App loads bytes and passes them to API/viewer. |
| `getDocumentPath`/implicit saving | Await export or handle `onExport`, then save an owned copy. |
| GlobalKey/private viewer state | Use the provided controller and typed state listeners. |
| Ad hoc feature launcher | Select supported semantic tools and configure host callbacks. |
| App-native picker inside PDF implementation | App supplies typed import callback and cancellation. |
| Private document handles across components | Transfer bytes; resource IDs remain owner-local. |

## AresScan+

The full protected viewer is the intended AresScan+ PDF editor, sharing processing
with the public API. Actual app migration is still incomplete. Its stamps,
watermarks, signatures, page grids, media overlays, scanning insertions, drafts,
read-aloud and save/history workflows require explicit mappings and regression
tests. Unsupported workflows must not silently disappear during migration.

App dependency files and the Edge/scanner SDK can be owned by another active
chat. Coordinate changes before altering app pubspec/lock files. This public
documentation preparation does not change the AresScan+ app or its owner licence.

## Versioned contracts

The `AresPdf...` model names preserve shared DTO identity with compatible
`PortablePdf...` aliases. Do not import implementation files or raw channels.
Use actual compiled capabilities when enabling newer options. Keep all package
members from the same release and review changelogs before updating a facade
independently of its native/Web members.
