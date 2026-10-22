# Spreadsheet scalar math in the protected PDF SDK

This local increment extends the existing opt-in numeric XLSX conversion profile with `ABS`, `SIGN`, `SQRT`, `INT`, `TRUNC`, `ROUND`, `ROUNDDOWN` and `ROUNDUP`. Processing stays in the compiled Core. It adds no script engine, external workbook access or network request. SDK licensing and document signature verification are unchanged.

## Check the compiled capability

`xlsxNumericFormulaEvaluation` identifies the existing numeric evaluator. The additional `xlsxScalarMathFunctions` flag identifies this extension. A newer Dart facade cannot add functions to an older binary. Without the new capability, older cores reject these function names; they do not silently use cached workbook values.

```dart
import 'package:ares_pdf/ares_pdf.dart';

final engine = await AresPdfApi.connect();
final capabilities = await engine.capabilities();
final nested = capabilities['core'];
final core = nested is Map ? nested : capabilities;
if (core['xlsxScalarMathFunctions'] != true) {
  throw StateError('This SDK binary lacks the scalar XLSX profile.');
}
```

Keep the existing local `ares_pdf` package and its two components together. This is a local, unsealed SDK increment; no publication or deployment is implied.

## Convert through the low-level API

Pass the workbook bytes and required font bytes explicitly. The application owns font loading and the returned PDF document. Close both the document and engine when finished.

```dart
import 'dart:typed_data';
import 'package:ares_pdf/ares_pdf.dart';

Future<Uint8List> convertWorkbook(
  Uint8List workbookBytes, {
  required Uint8List notoSansBytes,
}) async {
  final engine = await AresPdfApi.connect();
  try {
    final capabilities = await engine.capabilities();
    final nested = capabilities['core'];
    final core = nested is Map ? nested : capabilities;
    if (core['xlsxScalarMathFunctions'] != true) {
      throw StateError('The compiled scalar XLSX profile is unavailable.');
    }
    final converted = await engine.convertXlsx(
      workbookBytes,
      fonts: [AresPdfOfficeFontFamily('Noto Sans', regular: notoSansBytes)],
      options: const AresPdfOfficeOptions(evaluateNumericFormulas: true),
    );
    try {
      if (converted.report.formulaEvaluationProfile !=
          'same-sheet-numeric-scalar-v2') {
        throw StateError('Unexpected worksheet calculation policy.');
      }
      return await converted.document.exportBytes();
    } finally {
      await converted.document.close();
    }
  } finally {
    await engine.close();
  }
}
```

`formulaEvaluationProfile` is nullable for older or non-evaluating reports. It is distinct from the existing Office `profile` describing layout/conversion fidelity. `evaluatedFormulaCount` counts recalculated cells; `cachedFormulaCount` remains zero for successful explicit evaluation. The parser ignores a formula's stale saved `<v>` value when this option is enabled.

Evaluation remains opt-in. The default conversion policy does not automatically accept formula caches. `evaluateNumericFormulas` and `useCachedFormulaValues` cannot both be true. The numeric option applies to XLSX; do not pass it to low-level Word or PowerPoint conversion.

## Import through the full viewer

Your application supplies its own file picker and font bytes. Select the XLSX tool and configure its calculation policy before the user starts the import. `pickWorkbookBytes` below is application code returning workbook bytes or null to cancel.

```dart
final options = AresPdfViewerOptions(
  tools: const [AresPdfViewerTool.importXlsx],
  officeFonts: [AresPdfOfficeFontFamily('Noto Sans', regular: notoSansBytes)],
  officeOptions: const AresPdfOfficeOptions(evaluateNumericFormulas: true),
  hostCallbacks: AresPdfViewerHostCallbacks(
    onImportXlsx: (_) => pickWorkbookBytes(),
  ),
);
```

Use these options when constructing `AresPdfViewer`, or configure an existing controller before enabling the import tool. Office options are shared viewer settings; use the XLSX-specific policy with the XLSX tool rather than enabling it indiscriminately for Word/PowerPoint actions. Reconfiguration can cancel a pending host request, so do not change the settings from inside an already-running picker callback.

Successful import creates the imported PDF's own resource bank. Undo restores the original document and its registered font/image IDs. A failed conversion must retain the original bytes, revision and resource bank. Host request IDs, cancellation, document revisions and native licence checks still apply; importing through a host callback does not bypass these checks.

## Functions and examples

| Function | Arguments | Behavior | Example |
| --- | --- | --- | --- |
| `ABS` | One number | Absolute value | `ABS(-7.25)` → `7.25` |
| `SIGN` | One number | Negative, zero or positive sign | `SIGN(-12)` → `-1` |
| `SQRT` | One nonnegative number | Square root | `SQRT(144)` → `12` |
| `INT` | One number | Floor toward negative infinity | `INT(-4.3)` → `-5` |
| `TRUNC` | Number; optional decimal places | Discard digits toward zero | `TRUNC(-123.456,-1)` → `-120` |
| `ROUND` | Number and decimal places | Nearest, half away from zero | `ROUND(-2.5,0)` → `-3` |
| `ROUNDDOWN` | Number and decimal places | Discard toward zero | `ROUNDDOWN(-3.14159,3)` → `-3.141` |
| `ROUNDUP` | Number and decimal places | Round away from zero | `ROUNDUP(-3.14159,3)` → `-3.142` |

Functions can nest in supported arithmetic and aggregate expressions, for example `ROUND(AVERAGE(A1:B3),2)` or `SQRT(PRODUCT(A1,B1))`. Existing `SUM`, `AVERAGE`, `MIN`, `MAX`, `COUNT` and `PRODUCT` remain supported. Function names are case-insensitive. Same-sheet references, blank-cell arithmetic, numeric/Boolean worksheet values and existing dependency-cycle detection retain their bounded behavior. Unsupported text/error coercion fails explicitly.

The rounding and truncation distinctions follow Microsoft's function descriptions: [ROUND](https://support.microsoft.com/en-us/excel/functions/round-function), [rounding methods](https://support.microsoft.com/en-us/excel/round-a-number-to-the-decimal-places-i-want-in-excel), and [TRUNC](https://support.microsoft.com/en-us/excel/functions/trunc-function). This implementation remains a bounded profile, not an unrestricted Excel calculation engine.

## Calculation and PDF display precision

Dependency calculations retain IEEE754 values. They do not read back the text displayed in the PDF. Decimal-place operations use a fifteen-significant-digit decimal representation to decide discarded digits, including common boundaries such as `ROUND(1.005,2)` → `1.01`.

Formula values are displayed with at most fifteen significant digits. This avoids exposing binary tails such as `-3.1419999999999999` for the calculated value `-3.142`. Display precision does not reduce the precision of a dependent calculation. An independent regression checks that the display text is not fed back into dependency evaluation.

This does not establish Excel's complete precision, number-format, locale, currency, date or General-format fidelity. Literal cells and unsupported formatting retain their existing limits. A reported calculation policy is not a guarantee of full workbook fidelity.

## Errors and resource bounds

| Code | Meaning |
| --- | --- |
| `PDF_XLSX_FORMULA_ARGUMENTS` | Wrong scalar function arity or decimal-place argument outside the supported bound. |
| `PDF_XLSX_FORMULA_DOMAIN` | Unsupported numeric domain, including a negative square-root argument. |
| `PDF_XLSX_FORMULA_NONFINITE` | A calculation or rounded result cannot be represented as a finite supported value. |
| `PDF_XLSX_FORMULA_NON_NUMERIC` | The selected scalar argument cannot participate in supported numeric arithmetic. |
| `PDF_XLSX_FORMULA_CELL_ERROR` | A referenced error cell prevents calculation. |
| `PDF_XLSX_FORMULA_CYCLE` | A formula dependency cycle was encountered. |
| `PDF_XLSX_FORMULA_LIMIT` | Existing formula size, dependency, nesting or operation limits were exceeded. |
| `PDF_XLSX_FORMULA_POLICY` | Evaluation and cached-value modes were requested together. |
| `PDF_XLSX_FORMULA_UNSUPPORTED` | Formula grammar, function or reference is outside this profile. |

The raw decimal-place argument must lie in [−10,000, 10,000]; an allowed fractional value is then truncated toward zero. Very large requested results still fail if they are nonfinite. Existing limits include 8,192 bytes per formula, 512 KiB total formula text, 128 levels of nesting/dependency, 10,000 worksheet cells and one million evaluation steps. Scalar calls have at most two arguments. Range expressions remain supported only where the existing aggregate grammar accepts them. No formula can initiate file, script, network or external workbook access.

All conversion operations remain subject to the native SDK licence/trial policy. Real expiry denies new conversion while existing reader handles retain their supported page/raster operations.

## Acceptance and limits

The current local candidate passed eight canonical runs across the hosts listed in [platforms](platforms.md), with separate compiled-processing and independent saved-PDF checks. Windows runtime and macOS foreground UI remain pending. Full Excel/Office/PDF fidelity is not achieved; see [feature status](feature-status.md).
