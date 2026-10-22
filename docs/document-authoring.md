# Pages, graphics, text and layout

Use immutable `document.edit(List<AresPdfEdit>)` batches or
`controller.applyEdits(...)` on the current viewer. The complete constructors,
parameters and defaults are in [API reference](api-reference.md).

| Work | Public DTOs/operations |
| --- | --- |
| Pages | `AresPdfAddPage`, `AresPdfDeletePage`, `AresPdfRotatePage`, `AresPdfReorderPages`, `document.select`, `engine.merge` |
| Text | `AresPdfText`, `AresPdfParagraph`, `AresPdfRichParagraph`, `AresPdfTextRun` |
| Multi-block layout | `AresPdfFlow`, `AresPdfFlowParagraph`, `AresPdfFlowImage`, `AresPdfFlowTable` |
| Shapes | `AresPdfRectangle`, `AresPdfEllipse`, `AresPdfLine` |
| Tables | `AresPdfTable`, flow-table blocks and supported pagination options |
| Images | `AresPdfImage`, `registerImageRgba`, JPEG input |
| Existing supported objects | `pageObjects`, `AresPdfDeleteObject`, `AresPdfTransformObject` |

## Coordinates

PDF coordinates normally use points and unrotated PDF user space. The reference
defines the supported bounds/origin for each operation. Page numbers are one-based;
page insertion index is zero-based. Cropped/rotated pages require explicit
coordinate interpretation. Annotation DTOs can opt into displayed top-left
coordinates when the compiled capability supports it; do not apply that policy
to every other edit by assumption.

## Fonts and images

```dart
final fontId = await document.registerFont(fontBytes);
final imageId = await document.registerImageRgba(
  rgbaBytes,
  width: imageWidth,
  height: imageHeight,
);
```

These IDs belong to this owner/resource lineage. Use them only in operations
that accept IDs from that owner. Supply valid TrueType faces and correctly sized
RGBA bytes. Registered resources persist through supported native snapshots and
undo; they are not portable serialization handles. Font selection, licensing
and app font loading remain your responsibility.

## Layout scope

The compiled subset supports plain/mixed-run paragraphs, wrapping, explicit
first-line/hanging indents, tables and flow blocks including supported images.
Layout is bounded; general complex-script shaping, every typography feature,
all table pagination policies and complete Word fidelity are not established.
Unsupported input/options must reject explicitly rather than silently claim
fidelity. Check a saved output independently for your document corpus.

## Editing and preservation

General existing-content editing, tagged-structure preservation, arbitrary form
XObjects, layers, redaction and every merge structure family are not completed
by exposing `pageObjects` or a typed edit. Do not describe drawing a rectangle
over text as permanent redaction. Modifying a signed PDF can invalidate signatures;
optimize/author before signing unless using the specific incremental approval path.

Every result document must be closed. See [ownership](low-level-api.md),
[viewer history](viewer-editor.md) and [feature coverage](feature-status.md).
