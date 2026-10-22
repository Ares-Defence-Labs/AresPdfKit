# Compiled Office conversion

The SDK includes bounded DOCX, XLSX and PPTX conversion in its compiled Core.
App callbacks provide file bytes; they are not the conversion implementation.
Legacy binary DOC/XLS/PPT, general RTF, HTML, XPS and full Office fidelity are not
promised by these methods.

```dart
final converted = await engine.convertDocx(
  docxBytes,
  fonts: [AresPdfOfficeFontFamily('Noto Sans', regular: notoSansBytes)],
);
try {
  final report = converted.report;
  final pdf = await converted.document.exportBytes();
  // Keep/report the fidelity information and save the owned PDF bytes.
} finally {
  await converted.document.close();
}
```

`convertXlsx` and `convertPptx` use the same owned conversion result pattern.
`inspectOffice` reports the supported input profile without claiming conversion
success. Typed Office exceptions retain diagnostics/report data where supplied.
Check the complete report and unsupported-content findings rather than relying
only on a successful byte export.

## Supported subsets

| Input | Current bounded processing | Significant remaining gaps |
| --- | --- | --- |
| DOCX | Supported paragraphs/runs, explicit font faces, tables, paragraph indents and supported inline PNG/JPEG image flow/cropping | General Word pagination/sections, floating layout, advanced styles, fields and complex typography |
| XLSX | Supported worksheet grid, cells, bounded numeric formulas/caches under explicit policy and eight scalar functions | Full Excel formatting, dates, charts, cross-sheet/external formulas, macros and general workbook fidelity |
| PPTX | Supported positioned slide boxes and embedded JPEG/static PNG pictures | Master/theme fidelity, all shapes, charts, effects, animations and general PowerPoint rendering |

These descriptions identify a subset, not a claim that every document using
these constructs converts identically to Office. Native parsers reject unsupported
content and enforce archive/XML/resource bounds. No macro/script is executed.

## Fonts

Supply every required regular/bold/italic/bold-italic TrueType face explicitly.
The SDK does not read arbitrary OS fonts or synthesize a missing face as a fidelity
guarantee. Your application owns font licences and byte loading. Common viewer
limits are 16 MiB per face, 64 MiB total and at most 16 families.

## XLSX policy

Numeric evaluation is opt-in:

```dart
const options = AresPdfOfficeOptions(evaluateNumericFormulas: true);
```

Do not combine it with `useCachedFormulaValues`. Saved formula caches may be stale;
they are ignored during explicit evaluation. The returned report distinguishes
layout `profile`, nullable `formulaEvaluationProfile`, evaluated cells and cached
cells. Read [spreadsheet math](spreadsheet-math.md) for supported functions,
precision, exact errors and bounds. XLSX numeric options should not be applied
indiscriminately to DOCX/PPTX conversion.

## Viewer import

Select `importDocx`, `importXlsx` or `importPptx`; configure the corresponding host
callback, `officeFonts` and `officeOptions` before starting the action. Success
installs a new document/resource bank; undo restores the original. Failed import
must leave the original readable document/resources/history intact. Picker
cancellation and stale responses are handled through [host contexts](host-callbacks.md).
