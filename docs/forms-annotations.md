# Forms and annotations

## AcroForms

The current typed contract exposes `formFields()`, `AresPdfAddFormField`,
`fillForm`, `resetForm` and `flattenForm`. Supported categories include text,
checkbox, choice/radio and unsigned signature fields, subject to the exact DTO
and compiled profile. See the reference for options and constraints.

```dart
final fields = await document.formFields();
final filled = await document.fillForm({'customerName': 'Ada'});
try {
  final output = await filled.exportBytes();
  // Save output through app storage.
} finally {
  await filled.close();
}
```

Use exact fully qualified field names from inspection. Supply a registered font
for text outside supported defaults. Keep values compatible with the field type,
choice exports, read-only policy and constraints. Filling returns a snapshot;
it does not mutate the original owner.

Reset supported named fields or all fields, then export the returned document.
Flattening writes supported appearances into page content and removes supported
interactivity. Confirm the saved appearance with an independent reader. General
XFA, JavaScript calculations, every appearance stream and full form fidelity
remain incomplete. Creating an unsigned signature field does not sign a PDF.

The viewer's `formFields` tool exposes the supported native workflow. Hiding
the tool does not change low-level permissions or add unsupported form behavior.

## Annotations

Inspect `pageAnnotations(page)`. Add supported annotation kinds through
`AresPdfAddAnnotation`, edit supported contents with `AresPdfAnnotationContents`,
and remove by the documented annotation operation. See each constructor in the
[reference](api-reference.md).

Annotation metadata, appearance and page-content text are separate. Note text
is not automatically ordinary page content. The current saved-note profile
retains supported note appearances on export/reopen. Markup uses a bounded
rectangle/quad profile; arbitrary geometry and every annotation subtype are not
implied.

Use `displayCoordinates: true` only with the actual compiled
`displayAnnotationCoordinates` capability and its documented top-left displayed
coordinate system. Native code accounts for crop origins/page rotation. Normal
PDF-coordinate operations must stay within the supported visible region.

Typed viewer annotation edits participate in native history/revision checks.
Read-only/protected readers and expired licences still reject advanced edits.
