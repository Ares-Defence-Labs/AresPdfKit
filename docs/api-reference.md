# Canonical PDF SDK API reference

Generated from a frozen local candidate. Import `package:ares_pdf/ares_pdf.dart`. These signatures describe the local public contract; they do not establish complete Syncfusion parity, Windows runtime acceptance, paid-licence acceptance or publication readiness. Model aliases below preserve one shared DTO identity and remain compatible with their PortablePdf names. All PDF processing, rendering and licence enforcement remain compiled. See the canonical integration guide for ownership, platform limits and runtime evidence.

## AresPdfAddAnnotation

Public model alias of `PortablePdfAddAnnotation`. The shape below documents the alias.

Create a genuine PDF annotation, retained when saving and reopening.
Markup uses one rectangular quad. PDF coordinates use unrotated user space
and must lie within the visible crop/media intersection. With [displayCoordinates],
coordinates are points measured from the displayed page's top-left corner;
the compiled core accounts for page rotation and nonzero crop origins.
Display coordinates require the `displayAnnotationCoordinates` capability.
Note text is annotation metadata; its icon is rendered by the PDF engine.

```dart
class AresPdfAddAnnotation extends AresPdfEdit {
  const AresPdfAddAnnotation({
    required this.page,
    required this.kind,
    required this.x,
    required this.y,
    required this.width,
    required this.height,
    this.displayCoordinates = false,
    this.contents = '',
    this.author = '',
    this.color = const AresPdfColor(255, 210, 0),
  });
  final int page;
  final AresPdfAnnotationKind kind;
  final bool displayCoordinates;
  final double x, y, width, height;
  final String contents, author;
  final AresPdfColor color;
  Map<String, Object?> toJson();
}
```

## AresPdfAddFormField

Public model alias of `PortablePdfAddFormField`. The shape below documents the alias.

Add a flat, uniquely named AcroForm field. Dots in new names are rejected;
existing hierarchical names can be inspected and filled by qualified name.
Unicode appearances require a registered TrueType [fontId]. Bounds are PDF
user space inside the crop box. Required flags describe form semantics; the
SDK does not execute JavaScript validators. Edits return a new document.

```dart
class AresPdfAddFormField extends AresPdfEdit {
  const AresPdfAddFormField({
    required this.name,
    required this.kind,
    this.page = 1,
    this.bounds = const AresPdfBounds(0, 0, 100, 24),
    this.value,
    this.label,
    this.fontId,
    this.fontSize = 12,
    this.readOnly = false,
    this.required = false,
    this.multiline = false,
    this.password = false,
    this.multiple = false,
    this.maxLength,
    this.alignment = AresPdfTextAlign.left,
    this.options = const [],
    this.radioOptions = const [],
  });
  final String name;
  final String? label;
  final AresPdfFormKind kind;
  final int page;
  final AresPdfBounds bounds;
  final Object? value;
  final int? fontId, maxLength;
  final double fontSize;
  final bool readOnly, required, multiline, password, multiple;
  final AresPdfTextAlign alignment;
  final List<AresPdfFormOption> options;
  final List<AresPdfRadioOption> radioOptions;
  Map<String, Object?> toJson();
}
```

## AresPdfAddPage

Public model alias of `PortablePdfAddPage`. The shape below documents the alias.

Insert a blank page at a zero-based index in the current batch state.

```dart
class AresPdfAddPage extends AresPdfEdit {
  const AresPdfAddPage({
    required this.index,
    this.width = 612,
    this.height = 792,
  });
  final int index;
  final double width, height;
  Map<String, Object?> toJson();
}
```

## AresPdfAnnotation

Public model alias of `PortablePdfAnnotation`. The shape below documents the alias.

Saved annotation metadata. Indices change after deletion; inspect again.
[subtype] is the ISO/PDFium numeric subtype, preserving unknown input kinds.

```dart
class AresPdfAnnotation {
  const AresPdfAnnotation(
    this.index,
    this.subtype,
    this.bounds,
    this.contents,
    this.author,
  );
  final int index, subtype;
  final AresPdfBounds? bounds;
  final String contents, author;
  factory AresPdfAnnotation.fromJson(Map value);
}
```

## AresPdfAnnotationContents

Public model alias of `PortablePdfAnnotationContents`. The shape below documents the alias.

Update saved annotation text/author without flattening the annotation.

```dart
class AresPdfAnnotationContents extends AresPdfEdit {
  const AresPdfAnnotationContents({
    required this.page,
    required this.annotationIndex,
    required this.contents,
    this.author,
  });
  final int page, annotationIndex;
  final String contents;
  final String? author;
  Map<String, Object?> toJson();
}
```

## AresPdfAnnotationKind

Public model alias of `PortablePdfAnnotationKind`. The shape below documents the alias.

PDF annotation kinds supported for creation by the compiled core.

```dart
enum AresPdfAnnotationKind {note, highlight, underline, strikeout, squiggly, square}
```

## AresPdfApi

```dart
abstract final class AresPdfApi {
  /// Connecting admits normal API use and can start the native process trial.
  /// Platform selection and native ownership are internal; no library override.
  static Future<AresPdfEngine> connect({
    AresPdfRuntimeOptions options = const AresPdfRuntimeOptions(),
  });
}
```

## AresPdfAttachmentExport

Already-authorized copied output; the host owns saving and save errors.

```dart
class AresPdfAttachmentExport {
  AresPdfAttachmentExport({
    required this.context,
    required this.name,
    required Uint8List bytes,
  });
  final AresPdfHostRequestContext context;
  final String name;
  Uint8List get bytes;
}
```

## AresPdfAttachmentInfo

Public model alias of `PortablePdfAttachmentInfo`. The shape below documents the alias.

An embedded file's metadata. Indices change after attachment deletion.

```dart
class AresPdfAttachmentInfo {
  const AresPdfAttachmentInfo(this.index, this.name, this.byteLength);
  final int index, byteLength;
  final String name;
  factory AresPdfAttachmentInfo.fromJson(Map value);
}
```

## AresPdfBookmark

Public model alias of `PortablePdfBookmark`. The shape below documents the alias.

A bookmark with a Unicode title and an internal page destination.

Page numbers are one-based. A null page means an unresolved/external input
destination. Coordinates use unrotated PDF points; XYZ zoom is a multiplier
(1 means 100%; null/zero inherits the reader's current value).

```dart
class AresPdfBookmark {
  const AresPdfBookmark({
    required this.title,
    required this.page,
    this.children = const [],
    this.expanded = true,
    this.view = AresPdfDestinationView.fit,
    this.left,
    this.top,
    this.right,
    this.bottom,
    this.zoom,
  });
  final String title;
  final int? page;
  final List<AresPdfBookmark> children;
  final bool expanded;
  final AresPdfDestinationView view;
  final double? left, top, right, bottom, zoom;
  factory AresPdfBookmark.fromJson(Map value);
}
```

## AresPdfBounds

Public model alias of `PortablePdfBounds`. The shape below documents the alias.

Object geometry in the unrotated page's PDF coordinate system.

```dart
class AresPdfBounds {
  const AresPdfBounds(this.left, this.bottom, this.right, this.top);
  final double left, bottom, right, top;
}
```

## AresPdfColor

Public model alias of `PortablePdfColor`. The shape below documents the alias.

An RGBA color. Every channel must be an integer from 0 through 255.

```dart
class AresPdfColor {
  const AresPdfColor(this.red, this.green, this.blue, [this.alpha = 255]);
  static const black = AresPdfColor(0, 0, 0);
  static const white = AresPdfColor(255, 255, 255);
  final int red;
  final int green;
  final int blue;
  final int alpha;
  List<int> toJson();
}
```

## AresPdfDeleteAnnotation

Public model alias of `PortablePdfDeleteAnnotation`. The shape below documents the alias.

Delete the annotation at the inspected, zero-based index atomically.

```dart
class AresPdfDeleteAnnotation extends AresPdfEdit {
  const AresPdfDeleteAnnotation({
    required this.page,
    required this.annotationIndex,
  });
  final int page, annotationIndex;
  Map<String, Object?> toJson();
}
```

## AresPdfDeleteAttachment

Public model alias of `PortablePdfDeleteAttachment`. The shape below documents the alias.

Remove an embedded file from the attachment list on a new document.
This does not securely erase all copies of its bytes from the PDF.

```dart
class AresPdfDeleteAttachment extends AresPdfEdit {
  const AresPdfDeleteAttachment(this.attachmentIndex);
  final int attachmentIndex;
  Map<String, Object?> toJson();
}
```

## AresPdfDeleteObject

Public model alias of `PortablePdfDeleteObject`. The shape below documents the alias.

Remove a top-level page object from visible page content.
This is not secure redaction or proof that sensitive bytes are unrecoverable.

```dart
class AresPdfDeleteObject extends AresPdfEdit {
  const AresPdfDeleteObject({
    required this.page,
    required this.objectIndex,
  });
  final int page, objectIndex;
  Map<String, Object?> toJson();
}
```

## AresPdfDeletePage

Public model alias of `PortablePdfDeletePage`. The shape below documents the alias.

Delete a page. Deleting the final page is rejected.

```dart
class AresPdfDeletePage extends AresPdfEdit {
  const AresPdfDeletePage(this.page);
  final int page;
  Map<String, Object?> toJson();
}
```

## AresPdfDestinationView

Public model alias of `PortablePdfDestinationView`. The shape below documents the alias.

Standard PDF destination views. [unknown] represents an unsupported input
destination and cannot be written back as a new bookmark destination.

```dart
enum AresPdfDestinationView {fit, fitWidth, fitHeight, xyz, fitRectangle, fitBoundingBox, fitBoundingBoxWidth, fitBoundingBoxHeight, unknown}
```

## AresPdfDocument

```dart
abstract interface class AresPdfDocument {
  /// True after this snapshot closes or its owning engine is disposed.
  bool get isClosed;
  /// Read copied document metadata, outlines and supported AcroForm descriptors.
  /// Returned values are snapshots; refresh after an edit to see new state.
  Future<AresPdfDocumentInfo> documentInfo();
  Future<Map<String, String>> metadata();
  Future<List<AresPdfFormField>> formFields();
  /// Copy an embeddable TrueType face (at most 16 MiB) into this resource set.
  /// Returns a font ID for Unicode authoring, forms and layout. Restricted
  /// embedding rights, unsupported fonts or missing glyphs fail in the core.
  Future<int> registerFont(Uint8List bytes);
  /// Copy tightly packed, straight RGBA pixels with exactly width*height*4 bytes.
  /// Dimensions must be positive; the compiled limit is 32 million pixels.
  /// The returned ID can be used by this snapshot and its edit descendants.
  Future<int> registerImageRgba(
    Uint8List pixels, {
    required int width,
    required int height,
  });
  /// Return a new PDF containing a copied binary attachment with [name].
  /// Empty files are supported. Names, file counts and per-file sizes are bounded;
  /// duplicate names and unsupported attachment structures fail atomically.
  Future<AresPdfDocument> attach(String name, Uint8List bytes);
  /// Enumerate copied attachment descriptors; indices are zero-based.
  Future<List<AresPdfAttachmentInfo>> attachments();
  /// Copy the attachment at a zero-based index from [attachments].
  /// Refresh descriptors after attachment edits; indices are snapshot-local.
  Future<Uint8List> attachmentBytes(int index);
  /// Inspect supported top-level objects on a one-based page.
  /// Object indices are zero-based and must be refreshed after object removal.
  /// This is not a recursive semantic or tagged-document content model.
  Future<List<AresPdfPageObject>> pageObjects(int page);
  /// Read supported saved annotations from a one-based page without mutation.
  Future<List<AresPdfAnnotation>> pageAnnotations(int page);
  /// Extract embedded text from one page; this does not perform OCR.
  /// Reading order and unsupported font mappings follow the PDF text layer.
  Future<String> extractText(int page);
  /// Search embedded text across this snapshot, returning bounds and indices.
  /// [caseSensitive] and [wholeWord] use the compiled renderer search rules,
  /// not a locale-aware tokenizer. Search requires authoring/extraction access.
  Future<List<AresPdfSearchMatch>> search(
    String query, {
    bool caseSensitive = false,
    bool wholeWord = false,
  });
  /// Number of pages; available after expiry for restricted reader handles.
  Future<int> pageCount();
  /// Rotated display dimensions in PDF points for a one-based page.
  Future<AresPdfSize> pageSize(int page);
  /// Copy a straight-alpha BGRA raster of a one-based page, including widget appearances.
  /// [dpi] must be positive, finite and at most 2400; output is capped at
  /// 32 million pixels. Basic/protected reader handles can render after expiry.
  Future<AresPdfRaster> render(int page, {double dpi = 96});
  /// Encode a rendered page through the compiled PNG path.
  Future<Uint8List> renderPng(int page, {double dpi = 96});
  /// Copy unique, one-based page numbers in the supplied order.
  Future<AresPdfDocument> select(List<int> pages);
  Future<AresPdfDocument> fillForm(
    Map<String, Object> values, {
    int? fontId,
    double fontSize = 12,
  });
  Future<AresPdfDocument> resetForm({
    List<String> names = const [],
    int? fontId,
    double fontSize = 12,
  });
  Future<AresPdfDocument> flattenForm();
  Future<AresPdfDocument> setMetadata(Map<String, String?> values);
  /// Exact native source bytes; new/edited/protected readers reject explicitly.
  Future<Uint8List> copySourceBytes();
  /// Serialize a new caller-owned PDF buffer without mutating this snapshot.
  /// Reader-only encrypted handles cannot export plaintext. Serialization can
  /// change signed bytes: preserve original signed input or [PortablePdfAsyncEngine.signPdf] output
  /// for exact cryptographic export instead of re-saving it through a document.
  Future<Uint8List> exportBytes();
  /// Returns an independently owned snapshot. Unsupported edits fail natively.
  Future<AresPdfDocument> edit(List<AresPdfEdit> edits);
  /// Idempotently release this snapshot; cleanup is allowed after trial expiry.
  /// Descendant snapshots keep their own resource ownership. Other methods on
  /// this handle reject after close; the host still owns copied output buffers.
  Future<void> close();
}
```

## AresPdfDocumentInfo

Public model alias of `PortablePdfDocumentInfo`. The shape below documents the alias.

Read-only document information from the compiled structural backend.
Non-string PDF Info values remain preserved but are omitted from [metadata].

```dart
class AresPdfDocumentInfo {
  const AresPdfDocumentInfo(
    this.metadata,
    this.bookmarks, [
    this.forms = const [],
    this.hasXfa = false,
  ]);
  final Map<String, String> metadata;
  final List<AresPdfBookmark> bookmarks;
  final List<AresPdfFormField> forms;
  /// XFA forms are preserved but are not supported for filling/flattening.
  final bool hasXfa;
  /// Export JSON-compatible form data without running scripts. Button and
  /// signature fields are excluded. No values are logged or transmitted.
  Map<String, Object> get formValues;
  String? get title;
  String? get author;
  String? get subject;
  factory AresPdfDocumentInfo.fromJson(Map value);
}
```

## AresPdfEdit

Public model alias of `PortablePdfEdit`. The shape below documents the alias.

An edit applied to a new document as part of an atomic batch.

Page numbers are one-based. Coordinates use PDF points with the origin at
the unrotated page's bottom left. These commands do not mutate the input.

```dart
sealed class AresPdfEdit {
  const AresPdfEdit();
  Map<String, Object?> toJson();
}
```

## AresPdfEllipse

Public model alias of `PortablePdfEllipse`. The shape below documents the alias.

Draw a vector ellipse with four cubic Bezier segments inside the bounds.
Width/height use unrotated PDF points; fill and stroke follow rectangle rules.
Requires the compiled ellipseDrawing capability.

```dart
class AresPdfEllipse extends AresPdfEdit {
  const AresPdfEllipse({
    required this.page,
    required this.x,
    required this.y,
    required this.width,
    required this.height,
    this.fill,
    this.stroke = AresPdfColor.black,
    this.strokeWidth = 1,
  });
  final int page;
  final double x, y, width, height, strokeWidth;
  final AresPdfColor? fill, stroke;
  Map<String, Object?> toJson();
}
```

## AresPdfEncryption

Public model alias of `PortablePdfEncryption`. The shape below documents the alias.

AES-256 revision-6 password protection for an exported copy.

Passwords are UTF-8, at most 127 encoded bytes, without NUL. The owner
password must be nonempty. An explicitly empty user password permits opening
without a prompt while retaining owner permission settings. Metadata is
encrypted. Passwords are never included in error messages or SDK logs.

```dart
class AresPdfEncryption {
  const AresPdfEncryption({
    required this.ownerPassword,
    required this.userPassword,
    this.allowCopy = true,
    this.allowAssembly = true,
    this.allowAnnotations = true,
    this.allowFormFilling = true,
    this.allowModification = true,
    this.printing = AresPdfPrintPermission.full,
  });
  final String ownerPassword, userPassword;
  final bool allowCopy, allowAssembly, allowAnnotations;
  final bool allowFormFilling, allowModification;
  final AresPdfPrintPermission printing;
  Map<String, Object?> toJson();
}
```

## AresPdfEngine

Owner-bound byte API. PDF work, quotas and entitlement remain compiled.
This intentionally describes a subset of PortablePdfAsyncEngine.

```dart
abstract interface class AresPdfEngine {
  /// Whether disposal has started; subsequent requests reject.
  bool get isClosed;
  /// Feature flags reported by this binary, not a paid-entitlement verdict.
  /// Check before exposing optional workflows; older local binaries may lack
  /// newer flags and entry points.
  Future<Map<String, Object?>> capabilities();
  /// Read actual compiled host identity diagnostics; never supplies or changes
  /// an application ID or entitlement. `available:false` is explicit when an
  /// OS namespace/adapter/thread context is unavailable. Publisher verification
  /// is separate from a bundle ID, declared application ID or web origin.
  /// Requires applicationContext; this query does not start/reset the trial.
  Future<Map<String, Object?>> applicationContext();
  /// Current compiled entitlement and remaining monotonic trial, without I/O.
  /// Reader-only handles retain basic viewing when the trial reaches zero.
  Future<AresPdfLicenceState> licenceState();
  /// Verify the developer-embedded signed string entirely offline.
  /// Returns false for invalid/unsupported grants, retaining trial restrictions;
  /// it never resets the current compiled runtime trial deadline.
  Future<bool> activate(String signedKey);
  /// Export AES-256 R6 bytes from unencrypted input without mutating it.
  /// To change existing protection, explicitly owner-decrypt first.
  Future<Uint8List> encrypt(Uint8List bytes, AresPdfEncryption protection);
  /// Explicit owner-authorized decryption; user passwords alone are rejected.
  Future<Uint8List> decrypt(Uint8List bytes, {required String ownerPassword});
  /// Rewrite an unsigned, unencrypted PDF with lossless stream compression.
  /// [linearize] additionally produces fast-web-view output; [objectStreams]
  /// controls PDF object-stream generation. No image downsampling occurs and
  /// smaller output is not guaranteed. Re-sign only after this transformation.
  /// Signed/prepared/certified inputs reject with PDF_SIGNED_REWRITE_REQUIRED;
  /// encrypted input needs explicit owner decryption. Input remains unchanged.
  /// Requires losslessOptimization and, when requested, linearizedOutput.
  Future<Uint8List> optimize(
    Uint8List bytes, {
    bool linearize = false,
    bool recompressFlate = true,
    bool objectStreams = true,
  });
  /// Produce separate signed bytes using a caller-supplied PKCS#12 identity.
  /// Sign after editing. Set options.incremental to append an approval while
  /// preserving earlier signed bytes; requires incrementalSignatures. TSA
  /// issuance is separate. External CMS uses preparation/completion methods.
  Future<Uint8List> signPdf(
    Uint8List bytes,
    AresPdfSigningIdentity identity, {
    AresPdfSigningOptions options = const AresPdfSigningOptions(),
  });
  /// Prepare a PDF and copied exact byte ranges for an external
  /// detached CMS/CAdES producer. No private key is passed into the SDK.
  /// Requires externalCmsSigning. Signed input additionally requires explicit
  /// options.incremental and incrementalSignatures; certified input is rejected;
  /// encrypted input needs explicit owner decryption first. Keep the prepared
  /// PDF unchanged, and do not use it as a completed signed document.
  Future<AresPdfExternalSignatureRequest> prepareExternalSignature(
    Uint8List bytes, {
    AresPdfSigningOptions options = const AresPdfSigningOptions(),
  });
  /// Validate and embed external detached DER CMS in its unchanged preparation.
  /// The reserved CMS capacity is 32 KiB. Completion checks cryptographic
  /// integrity, strong key/digest policies and whole-file byte-range coverage.
  /// It does not grant signer/TSA trust, revocation, LTV or certification status;
  /// call verifySignatures with explicit roots for those supported results.
  /// Every phase enforces the current native licence/trial independently.
  Future<Uint8List> completeExternalSignature(
    Uint8List preparedPdf,
    Uint8List cms,
  );
  /// Create a DER RFC3161 request over one detached CMS signature value.
  /// Requires signatureTimestampIssuance. SHA-256 and a fresh 128-bit nonce are
  /// generated in compiled code. The optional policy is a numeric OID. Keep this
  /// exact request and CMS unchanged; the host sends the DER to its chosen TSA.
  Future<Uint8List> createSignatureTimestampRequest(
    Uint8List cms, {
    String policyOid = '',
  });
  /// Validate a full DER TimeStampResp against the retained request, explicit TSA
  /// roots, nonce, policy, imprint, token signature, EKU and generation time.
  /// Return new CMS bytes containing the timestamp; no network or private key
  /// is used. Only one signer and no existing timestamp are supported. The
  /// final CMS must fit 32 KiB. Current native licensing is checked again.
  /// Complete the unchanged PDF preparation with these CMS bytes afterward.
  Future<Uint8List> completeSignatureTimestamp(
    Uint8List cms, {
    required Uint8List request,
    required Uint8List response,
    required String timestampTrustAnchorsPem,
  });
  /// Verify PDF integrity, revision coverage, signer trust and RFC3161 tokens
  /// independently. Signer and TSA roots are explicit and separate. No system
  /// trust store, network, revocation or LTV check is implied by a valid result.
  Future<AresPdfSignatureReport> verifySignatures(
    Uint8List bytes, {
    AresPdfSignatureVerificationOptions options =
        const AresPdfSignatureVerificationOptions(),
  });
  /// Create one blank page in PDF points (72 per inch), owned by this engine.
  /// Width/height must be finite and pass the compiled page-size limits.
  Future<AresPdfDocument> createDocument({
    double width = 612,
    double height = 792,
  });
  /// Open a copied unencrypted PDF without executing document JavaScript.
  /// [forViewing] creates a restricted basic reader: only count, size and raw
  /// raster remain available without entitlement. It does not grant editing or
  /// export. Use [openProtectedReader] for open-password encrypted reading, or
  /// [decrypt] with owner authorization before opening an editable copy.
  Future<AresPdfDocument> open(
    Uint8List bytes, {
    AresPdfOpenMode mode = AresPdfOpenMode.editor,
  });
  /// Open encrypted input for page count, geometry and raster only. No password
  /// is retained by the core. All edits/extraction/export need owner unlock.
  Future<AresPdfDocument> openProtectedReader(
    Uint8List bytes, {
    required String password,
  });
  /// Create one image-only page retaining the original JPEG DCT data.
  /// [dpi] determines page dimensions; EXIF orientation is not applied. Input
  /// is bounded to 64 MiB and 32 million pixels. Close the returned document.
  Future<AresPdfDocument> fromJpeg(Uint8List bytes, {double dpi = 96});
  /// Inputs must be open snapshots from this owner; sources stay unchanged.
  Future<AresPdfDocument> merge(List<AresPdfDocument> documents);
  /// Inspect a bounded OOXML package without writing files or fetching resources.
  Future<AresPdfOfficeReport> inspectOffice(Uint8List bytes);
  /// Convert strict DOCX text and plain uniform tables into a new owned PDF.
  /// Explicit [fonts] supply permitted embedding faces; no system-font discovery
  /// or Office installation is used. Unsupported content throws
  /// [AresPdfOfficeException] with diagnostics. Close the returned document;
  /// the input and other open PDFs remain unchanged.
  Future<AresPdfOfficeConversion<AresPdfDocument>> convertDocx(
    Uint8List bytes, {
    List<AresPdfOfficeFontFamily> fonts = const [],
    AresPdfOfficeOptions options = const AresPdfOfficeOptions(),
  });
  /// Convert XLSX cell values into a paginated coordinate grid. This profile
  /// rejects unsupported styles/drawings. Formula caches require explicit
  /// useCachedFormulaValues; capable cores can instead evaluate the bounded
  /// same-sheet numeric profile with evaluateNumericFormulas. These options are
  /// mutually exclusive. Numeric evaluation never falls back to stale caches.
  Future<AresPdfOfficeConversion<AresPdfDocument>> convertXlsx(
    Uint8List bytes, {
    List<AresPdfOfficeFontFamily> fonts = const [],
    AresPdfOfficeOptions options = const AresPdfOfficeOptions(),
  });
  /// Convert explicit positioned PPTX slide boxes into a new owned PDF.
  /// The strict profile supports RGB backgrounds, vector rectangles/ellipses
  /// and fully specified text. Unsupported inheritance, images, charts,
  /// placeholders or other content block conversion with a diagnostic report.
  /// Text metrics use the compiled fonts, not PowerPoint layout fidelity.
  Future<AresPdfOfficeConversion<AresPdfDocument>> convertPptx(
    Uint8List bytes, {
    List<AresPdfOfficeFontFamily> fonts = const [],
    AresPdfOfficeOptions options = const AresPdfOfficeOptions(),
  });
  /// Cached future acknowledges native ownership cleanup; never resets a trial.
  Future<void> close();
}
```

## AresPdfException

```dart
class AresPdfException implements Exception {
  const AresPdfException(this.code, {this.details});
  final String code;
  /// Copied native diagnostics, when supplied; never an entitlement verdict.
  final Object? details;
  String toString();
}
```

## AresPdfExportCallback

Receives an already-authorized copied PDF. The host owns saving and errors.

```dart
typedef AresPdfExportCallback = Future<void> Function(Uint8List bytes);
```

## AresPdfExternalCmsRequest

A detached DER CMS/CAdES object must sign these exact content bytes using
the compiled SHA-256 profile. A raw RSA/ECDSA signature is insufficient.

```dart
class AresPdfExternalCmsRequest {
  AresPdfExternalCmsRequest(this.context, Uint8List contentToSign);
  final AresPdfHostRequestContext context;
  Uint8List get contentToSign;
}
```

## AresPdfExternalSignatureRequest

Public model alias of `PortablePdfExternalSignatureRequest`. The shape below documents the alias.

Owned PDF preparation and the exact bytes a detached CMS producer must sign.
Do not edit/re-save [preparedPdf] between phases. [contentToSign] is PDF byte-
range content, not a hash or DER signed-attributes set. A signing service must
construct a detached CMS/CAdES object; a bare RSA/ECDSA signature is insufficient.
Getters return copies. No identity, private key or signing-service URL is kept.

```dart
class AresPdfExternalSignatureRequest {
  AresPdfExternalSignatureRequest(
    Uint8List preparedPdf,
    Uint8List contentToSign,
  );
  Uint8List get preparedPdf;
  Uint8List get contentToSign;
}
```

## AresPdfFillForm

Public model alias of `PortablePdfFillForm`. The shape below documents the alias.

Fill exact qualified names atomically. Text/choice values are strings,
checkboxes are bools and multi-select lists accept `List<String>`. Radio values
identify a widget's on-state; empty string clears the group. Read-only fields
reject edits. Unicode text/choice appearances require [fontId]. Values remain
complete even when the widget viewport clips their visible appearance.

```dart
class AresPdfFillForm extends AresPdfEdit {
  const AresPdfFillForm(this.values, {this.fontId, this.fontSize = 12});
  final Map<String, Object> values;
  final int? fontId;
  final double fontSize;
  Map<String, Object?> toJson();
}
```

## AresPdfFlattenForm

Public model alias of `PortablePdfFlattenForm`. The shape below documents the alias.

Flatten genuine widget appearances into page content and remove AcroForm.
Ordinary comments remain annotations. Missing appearances and signed fields
fail rather than silently losing visual content or pretending signatures
remain valid. Flattening is not secure erasure of historical revisions.

```dart
class AresPdfFlattenForm extends AresPdfEdit {
  const AresPdfFlattenForm();
  Map<String, Object?> toJson();
}
```

## AresPdfFlow

Public model alias of `PortablePdfFlow`. The shape below documents the alias.

Flow styled paragraphs through a shared page frame in compiled code.
Each paragraph retains its own alignment, indents and spacing. Pagination
inserts matching pages and preserves subsequent existing pages. This initial
flow supports paragraphs and tables; floating objects, footnotes and
widow/orphan rules remain unfinished.

```dart
class AresPdfFlow extends AresPdfEdit {
  const AresPdfFlow({
    required this.page,
    required this.blocks,
    required this.x,
    required this.top,
    required this.bottom,
    required this.width,
    this.paginate = false,
  });
  final int page;
  final List<AresPdfFlowBlock> blocks;
  final double x, top, bottom, width;
  final bool paginate;
  Map<String, Object> toJson();
}
```

## AresPdfFlowBlock

Public model alias of `PortablePdfFlowBlock`. The shape below documents the alias.

A paragraph, image or table participating in one compiled layout cursor.

```dart
abstract interface class AresPdfFlowBlock {
  Map<String, Object?> toJson();
}
```

## AresPdfFlowImage

Public model alias of `PortablePdfFlowImage`. The shape below documents the alias.

A registered RGBA image kept intact in the compiled pagination cursor.
Width/height and spacing use PDF points. The image must fit a fresh frame;
it is never silently scaled, cropped or split. Explicit source cropping
requires flowImageCropping in addition to imagesInFlow.
Resource IDs belong to this document or its edit ancestor. Per flow, at most
128 placements and 32 million placed decoded pixels are allowed.

```dart
class AresPdfFlowImage implements AresPdfFlowBlock {
  const AresPdfFlowImage({
    required this.imageId,
    required this.width,
    required this.height,
    this.align = AresPdfFlowImageAlign.left,
    this.spaceBefore = 0,
    this.spaceAfter = 0,
    this.leftIndent = 0,
    this.rightIndent = 0,
    this.pageBreakBefore = false,
    this.cropLeft = 0,
    this.cropTop = 0,
    this.cropRight = 0,
    this.cropBottom = 0,
  });
  final int imageId;
  final double width, height, spaceBefore, spaceAfter, leftIndent, rightIndent;
  final AresPdfFlowImageAlign align;
  final bool pageBreakBefore;
  /// Explicit original-source fractions, requiring flowImageCropping.
  /// Each edge and each opposing sum is in 0..0.99; the target footprint
  /// remains width/height and participates normally in pagination.
  final double cropLeft, cropTop, cropRight, cropBottom;
  bool get hasCrop;
  Map<String, Object?> toJson();
}
```

## AresPdfFlowImageAlign

Public model alias of `PortablePdfFlowImageAlign`. The shape below documents the alias.

Horizontal alignment of an unscaled image within a flow frame.

```dart
enum AresPdfFlowImageAlign {left, center, right}
```

## AresPdfFlowParagraph

Public model alias of `PortablePdfFlowParagraph`. The shape below documents the alias.

A styled paragraph within a compiled flow. Spacing and indents use points.

```dart
class AresPdfFlowParagraph implements AresPdfFlowBlock {
  const AresPdfFlowParagraph(
    this.runs, {
    this.spaceBefore = 0,
    this.spaceAfter = 0,
    this.leftIndent = 0,
    this.rightIndent = 0,
    this.firstLineIndent = 0,
    this.pageBreakBefore = false,
    this.leading,
    this.align = AresPdfTextAlign.left,
  });
  final List<AresPdfTextRun> runs;
  final double spaceBefore, spaceAfter, leftIndent, rightIndent;
  /// Signed first-line offset from [leftIndent], in points. A hanging indent
  /// cannot extend beyond the enclosing flow frame's left edge.
  final double firstLineIndent;
  final bool pageBreakBefore;
  final double? leading;
  final AresPdfTextAlign align;
  Map<String, Object?> toJson();
}
```

## AresPdfFlowTable

Public model alias of `PortablePdfFlowTable`. The shape below documents the alias.

A compiled table participating in a common paragraph/table flow cursor.
Column widths are absolute points and must fit the flow frame after indents.
Each row uses a single text style; rich/merged cells remain unfinished.

```dart
class AresPdfFlowTable implements AresPdfFlowBlock {
  const AresPdfFlowTable({
    required this.rows,
    required this.columns,
    this.headerRows = 1,
    this.padding = 4,
    this.fontSize = 12,
    this.leading,
    this.font = 'Helvetica',
    this.fontId,
    this.align = AresPdfTextAlign.left,
    this.color = AresPdfColor.black,
    this.border = AresPdfColor.black,
    this.borderWidth = 0.5,
    this.headerFill = const AresPdfColor(225, 240, 238),
    this.cellFill,
    this.splitRows = false,
    this.spaceBefore = 0,
    this.spaceAfter = 0,
    this.leftIndent = 0,
    this.rightIndent = 0,
    this.pageBreakBefore = false,
  });
  final List<List<String>> rows;
  final List<double> columns;
  final int headerRows;
  final double padding, fontSize, borderWidth, spaceBefore, spaceAfter, leftIndent, rightIndent;
  final double? leading;
  final String font;
  final int? fontId;
  final AresPdfTextAlign align;
  final AresPdfColor color;
  final AresPdfColor? border, headerFill, cellFill;
  final bool splitRows, pageBreakBefore;
  Map<String, Object?> toJson();
}
```

## AresPdfFormField

Public model alias of `PortablePdfFormField`. The shape below documents the alias.

Read-only compiled inspection of a logical field, grouping radio widgets.
Values are String, bool, `List<String>`, or null. Flags retain the original PDF
bits. Inspection never runs form scripts, validation actions or submissions.

```dart
class AresPdfFormField {
  const AresPdfFormField({
    required this.name,
    required this.label,
    required this.kind,
    required this.flags,
    required this.value,
    required this.options,
    required this.widgets,
  });
  final String name, label;
  final AresPdfFormKind kind;
  final int flags;
  final Object? value;
  final List<AresPdfFormOption> options;
  final List<AresPdfFormWidget> widgets;
  bool get readOnly;
  bool get required;
  bool get multiline;
  bool get password;
  bool get multiple;
  String? get textValue;
  bool? get checked;
  factory AresPdfFormField.fromJson(Map v);
}
```

## AresPdfFormKind

Public model alias of `PortablePdfFormKind`. The shape below documents the alias.

Interactive AcroForm field categories. Signature fields here are unsigned
placeholders; creating one does not digitally sign a document.

```dart
enum AresPdfFormKind {text, checkbox, radio, combo, list, button, signature, unknown}
```

## AresPdfFormOption

Public model alias of `PortablePdfFormOption`. The shape below documents the alias.

A choice's stored export value and its user-visible label.

```dart
class AresPdfFormOption {
  const AresPdfFormOption(this.value, this.label);
  final String value, label;
  Map<String, Object?> toJson();
}
```

## AresPdfFormWidget

Public model alias of `PortablePdfFormWidget`. The shape below documents the alias.

A saved form widget's position. Null page means the widget is not reachable
from the document's page annotation arrays.

```dart
class AresPdfFormWidget {
  const AresPdfFormWidget(this.page, this.bounds);
  final int? page;
  final AresPdfBounds bounds;
}
```

## AresPdfHostRequestContext

Correlation supplied by the compiled viewer; never permission to process.
[cancelled] completes on replacement, reconfiguration, timeout or close.

```dart
class AresPdfHostRequestContext {
  const AresPdfHostRequestContext({
    required this.requestId,
    required this.configurationId,
    required this.documentRevision,
    required this.cancelled,
  });
  final String requestId, configurationId;
  final int documentRevision;
  final Future<void> cancelled;
}
```

## AresPdfImage

Public model alias of `PortablePdfImage`. The shape below documents the alias.

Draw copied RGBA image data registered with this document or its edit ancestor.
Image resources do not transfer across independently opened/merged documents.

```dart
class AresPdfImage extends AresPdfEdit {
  const AresPdfImage({
    required this.page,
    required this.imageId,
    required this.x,
    required this.y,
    required this.width,
    required this.height,
    this.rotationDegrees = 0,
    this.flipHorizontal = false,
    this.flipVertical = false,
    this.cropLeft = 0,
    this.cropTop = 0,
    this.cropRight = 0,
    this.cropBottom = 0,
  });
  final int page, imageId;
  final double x, y, width, height;
  /// Counterclockwise PDF-space rotation about the placed rectangle center.
  /// Requires imageTransforms when nonzero; each input coordinate/angle is
  /// finite and bounded to +/-14400. Flips apply in local axes before rotation.
  final double rotationDegrees;
  final bool flipHorizontal, flipVertical;
  /// Fractions removed from original source edges before fitting the rectangle.
  /// Each edge and each opposing-edge sum must be within 0..0.99. Cropping
  /// preserves source image data inside an isolated clip and requires imageCropping.
  final double cropLeft, cropTop, cropRight, cropBottom;
  bool get hasCrop;
  bool get hasTransform;
  Map<String, Object?> toJson();
}
```

## AresPdfImportedAttachment

A file name is a display suggestion, never a trusted filesystem path.

```dart
class AresPdfImportedAttachment {
  AresPdfImportedAttachment(this.name, Uint8List bytes);
  final String name;
  Uint8List get bytes;
}
```

## AresPdfLicenceState

```dart
class AresPdfLicenceState {
  const AresPdfLicenceState({
    required this.licensed,
    required this.trialRemaining,
  });
  final bool licensed;
  final Duration trialRemaining;
}
```

## AresPdfLine

Public model alias of `PortablePdfLine`. The shape below documents the alias.

Draw a vector line between two PDF coordinates.

```dart
class AresPdfLine extends AresPdfEdit {
  const AresPdfLine({
    required this.page,
    required this.x,
    required this.y,
    required this.x2,
    required this.y2,
    this.color = AresPdfColor.black,
    this.strokeWidth = 1,
  });
  final int page;
  final double x, y, x2, y2, strokeWidth;
  final AresPdfColor color;
  Map<String, Object?> toJson();
}
```

## AresPdfObjectType

Public model alias of `PortablePdfObjectType`. The shape below documents the alias.

```dart
enum AresPdfObjectType {unknown, text, path, image, shading, form}
```

## AresPdfOfficeConversion

Public model alias of `PortablePdfOfficeConversion`. The shape below documents the alias.

A successful conversion owns its document; close it when finished.

```dart
class AresPdfOfficeConversion<T> {
  const AresPdfOfficeConversion(this.document, this.report);
  final T document;
  final AresPdfOfficeReport report;
}
```

## AresPdfOfficeException

Public model alias of `PortablePdfOfficeException`. The shape below documents the alias.

Unsupported content retains the preflight report for actionable diagnostics.

```dart
class AresPdfOfficeException extends PortablePdfException {
  const AresPdfOfficeException(this.report);
  final AresPdfOfficeReport report;
}
```

## AresPdfOfficeFontFamily

Public model alias of `PortablePdfOfficeFontFamily`. The shape below documents the alias.

Explicit TrueType faces used by compiled Office conversion. No OS font lookup.
Supply each face used by the document; missing bold/italic is an error.

```dart
class AresPdfOfficeFontFamily {
  const AresPdfOfficeFontFamily(
    this.name, {
    required this.regular,
    this.bold,
    this.italic,
    this.boldItalic,
  });
  final String name;
  final Uint8List regular;
  final Uint8List? bold, italic, boldItalic;
}
```

## AresPdfOfficeOptions

Public model alias of `PortablePdfOfficeOptions`. The shape below documents the alias.

Strict Office conversion options. Font substitution and cached formulas
require explicit opt-in. Numeric formula evaluation is a separate bounded profile.

```dart
class AresPdfOfficeOptions {
  const AresPdfOfficeOptions({
    this.fallbackFamily = 'Helvetica',
    this.allowFontSubstitution = false,
    this.useCachedFormulaValues = false,
    this.evaluateNumericFormulas = false,
  });
  final String fallbackFamily;
  final bool allowFontSubstitution, useCachedFormulaValues;
  /// Recalculate the bounded same-sheet numeric XLSX profile locally. Requires
  /// xlsxNumericFormulaEvaluation; mutually exclusive with cached values.
  /// Unsupported formulas fail without falling back to a stale cached result.
  final bool evaluateNumericFormulas;
  Map<String, Object?> toJson();
}
```

## AresPdfOfficeReport

Public model alias of `PortablePdfOfficeReport`. The shape below documents the alias.

Diagnostic report from the compiled, memory-only OOXML parser.
[supported] covers the advertised conversion profile, not arbitrary Office
fidelity. Cached formula results can be stale and require explicit opt-in.

```dart
class AresPdfOfficeReport {
  AresPdfOfficeReport.fromJson(Map<String, dynamic> value);
  final String format, profile;
  /// Compiled worksheet-calculation policy, when supplied by the Core.
  /// Null for older/non-evaluating reports. This does not imply full Excel parity.
  final String? formulaEvaluationProfile;
  final bool supported;
  final List<Map<String, Object?>> diagnostics, fontSubstitutions;
  final List<String> fontFamilies;
  final int paragraphCount, sheetCount, cellCount, cachedFormulaCount, evaluatedFormulaCount, tableCount, slideCount, shapeCount, imageCount;
  /// Unique embedded image parts used by the bounded DOCX image profile.
  /// Older profiles omit this count and report zero.
  final int imageParts;
}
```

## AresPdfOpenMode

```dart
enum AresPdfOpenMode {editor, reader}
```

## AresPdfPageObject

Public model alias of `PortablePdfPageObject`. The shape below documents the alias.

A top-level page object. Form objects are not recursively expanded here.
Indices describe the inspected document and change when objects are removed.

```dart
class AresPdfPageObject {
  const AresPdfPageObject(this.index, this.type, this.bounds);
  final int index;
  final AresPdfObjectType type;
  final AresPdfBounds? bounds;
}
```

## AresPdfPagePngExport

```dart
class AresPdfPagePngExport {
  AresPdfPagePngExport({
    required this.context,
    required this.page,
    required Uint8List bytes,
  });
  final AresPdfHostRequestContext context;
  /// One-based source page.
  final int page;
  Uint8List get bytes;
}
```

## AresPdfParagraph

Public model alias of `PortablePdfParagraph`. The shape below documents the alias.

Wrap text to a rectangular frame using the selected font's native metrics.

Ordinary whitespace within a line collapses to one space; explicit newlines
start new lines. Long words split at glyph boundaries. [top] and [bottom]
are PDF y coordinates. If [paginate] is true, overflow inserts matching blank
pages immediately after this page. Otherwise overflow fails the whole batch.
Layout requires an unrotated page. Use a registered font for Unicode text.

```dart
class AresPdfParagraph extends AresPdfEdit {
  const AresPdfParagraph({
    required this.page,
    required this.text,
    required this.x,
    required this.top,
    required this.bottom,
    required this.width,
    this.fontSize = 12,
    this.leading,
    this.font = 'Helvetica',
    this.fontId,
    this.align = AresPdfTextAlign.left,
    this.color = AresPdfColor.black,
    this.paginate = false,
  });
  final int page;
  final String text, font;
  final double x, top, bottom, width, fontSize;
  /// Omit to use line spacing derived from the chosen font's actual metrics.
  final double? leading;
  final int? fontId;
  final AresPdfTextAlign align;
  final bool paginate;
  final AresPdfColor color;
  Map<String, Object?> toJson();
}
```

## AresPdfPrintPermission

Public model alias of `PortablePdfPrintPermission`. The shape below documents the alias.

Print permissions recorded in an AES-256 protected PDF.
Permissions depend on reader cooperation; the owner password authorizes
removing protection. Accessibility extraction remains enabled.

```dart
enum AresPdfPrintPermission {none, lowResolution, full}
```

## AresPdfRadioOption

Public model alias of `PortablePdfRadioOption`. The shape below documents the alias.

One radio widget. Bounds use unrotated PDF user space; pages are one-based.

```dart
class AresPdfRadioOption {
  const AresPdfRadioOption({
    required this.value,
    required this.page,
    required this.bounds,
  });
  final String value;
  final int page;
  final AresPdfBounds bounds;
  Map<String, Object?> toJson();
}
```

## AresPdfRaster

Public model alias of `PortablePdfRaster`. The shape below documents the alias.

An owned copy of rendered pixels in BGRA byte order, with straight alpha.

```dart
class AresPdfRaster {
  const AresPdfRaster(this.width, this.height, this.bgra);
  final int width;
  final int height;
  final Uint8List bgra;
}
```

## AresPdfRectangle

Public model alias of `PortablePdfRectangle`. The shape below documents the alias.

Draw a vector rectangle with fill, stroke, or both.

```dart
class AresPdfRectangle extends AresPdfEdit {
  const AresPdfRectangle({
    required this.page,
    required this.x,
    required this.y,
    required this.width,
    required this.height,
    this.fill,
    this.stroke = AresPdfColor.black,
    this.strokeWidth = 1,
  });
  final int page;
  final double x, y, width, height, strokeWidth;
  final AresPdfColor? fill, stroke;
  Map<String, Object?> toJson();
}
```

## AresPdfRemoveFormField

Public model alias of `PortablePdfRemoveFormField`. The shape below documents the alias.

Remove a logical field and its page widgets, retaining unrelated fields.

```dart
class AresPdfRemoveFormField extends AresPdfEdit {
  const AresPdfRemoveFormField(this.name);
  final String name;
  Map<String, Object?> toJson();
}
```

## AresPdfReorderPages

Public model alias of `PortablePdfReorderPages`. The shape below documents the alias.

Reorder every existing page using a complete one-based permutation.
Existing page objects, document destinations and registered resources remain
owned by the native immutable descendant. Requires `pageReordering`.

```dart
class AresPdfReorderPages extends AresPdfEdit {
  AresPdfReorderPages(List<int> pages);
  final List<int> pages;
  Map<String, Object?> toJson();
}
```

## AresPdfResetForm

Public model alias of `PortablePdfResetForm`. The shape below documents the alias.

Restore saved default values. Empty [names] resets all supported fields;
supplied names must exist. This explicit reset also restores read-only
defaults, while ordinary [AresPdfFillForm] rejects read-only edits.

```dart
class AresPdfResetForm extends AresPdfEdit {
  const AresPdfResetForm({
    this.names = const [],
    this.fontId,
    this.fontSize = 12,
  });
  final List<String> names;
  final int? fontId;
  final double fontSize;
  Map<String, Object?> toJson();
}
```

## AresPdfRevocationStatus

Public model alias of `PortablePdfRevocationStatus`. The shape below documents the alias.

Offline document-certificate revocation, separate from CMS integrity/trust.
Good/revoked are definitive only for the supplied complete issuer evidence.

```dart
enum AresPdfRevocationStatus {notEvaluated, good, revoked, stale, unknown, invalid; static AresPdfRevocationStatus fromJson(Object? value) {if (value == null) return notEvaluated; return values.firstWhere((status) => status.name == value, orElse: () => invalid);}}
```

## AresPdfRichParagraph

Public model alias of `PortablePdfRichParagraph`. The shape below documents the alias.

Lay out mixed fonts, sizes and colors with shared word wrapping and per-line
ascent/descent. Runs can split a word without introducing spaces. Ordinary
whitespace collapses; explicit newlines remain. Overflows reject atomically
or insert matching pages with [paginate]. Shaping, bidi, inline images,
semantic tags and widow/orphan control remain separate requirements.

```dart
class AresPdfRichParagraph extends AresPdfEdit {
  const AresPdfRichParagraph({
    required this.page,
    required this.runs,
    required this.x,
    required this.top,
    required this.bottom,
    required this.width,
    this.firstLineIndent = 0,
    this.leading,
    this.align = AresPdfTextAlign.left,
    this.paginate = false,
  });
  final int page;
  final List<AresPdfTextRun> runs;
  final double x, top, bottom, width;
  /// Signed points from the paragraph origin on its first line only. Negative
  /// values create a hanging indent and must remain inside the page frame.
  final double firstLineIndent;
  final double? leading;
  final AresPdfTextAlign align;
  final bool paginate;
  Map<String, Object?> toJson();
}
```

## AresPdfRotatePage

Public model alias of `PortablePdfRotatePage`. The shape below documents the alias.

Rotate clockwise by -3 through 3 quarter turns relative to current rotation.

```dart
class AresPdfRotatePage extends AresPdfEdit {
  const AresPdfRotatePage(this.page, this.quarterTurns);
  final int page, quarterTurns;
  Map<String, Object?> toJson();
}
```

## AresPdfRuntimeOptions

```dart
class AresPdfRuntimeOptions {
  const AresPdfRuntimeOptions({
    this.webRuntimeRoot =
        'assets/packages/ares_pdf_protected_web/assets/runtime/',
  });
  /// Same-origin compiled Web assets; never a native library or verifier override.
  final String webRuntimeRoot;
}
```

## AresPdfSearchMatch

Public model alias of `PortablePdfSearchMatch`. The shape below documents the alias.

An exact native text-search hit. Page is one-based; character positions are
PDFium text-page indices, not Dart string offsets. Bounds use PDF coordinates.

```dart
class AresPdfSearchMatch {
  const AresPdfSearchMatch(
    this.page,
    this.characterIndex,
    this.characterCount,
    this.bounds,
  );
  final int page, characterIndex, characterCount;
  final List<AresPdfBounds> bounds;
  factory AresPdfSearchMatch.fromJson(Map value);
}
```

## AresPdfSetBookmarks

Public model alias of `PortablePdfSetBookmarks`. The shape below documents the alias.

Replace the outline tree. An empty list clears it. The compiled writer
maintains parent/sibling links, destinations and expanded/closed counts.

```dart
class AresPdfSetBookmarks extends AresPdfEdit {
  const AresPdfSetBookmarks(this.items);
  final List<AresPdfBookmark> items;
  Map<String, Object?> toJson();
}
```

## AresPdfSetMetadata

Public model alias of `PortablePdfSetMetadata`. The shape below documents the alias.

Set/remove PDF Info strings atomically. Keys omit the leading PDF slash;
a null value removes that key. XMP packets are a separate document feature.

```dart
class AresPdfSetMetadata extends AresPdfEdit {
  const AresPdfSetMetadata({required this.values});
  factory AresPdfSetMetadata.standard({
    String? title,
    String? author,
    String? subject,
    String? keywords,
    String? creator,
    String? producer,
    Map<String, String?> extra = const {},
  });
  final Map<String, String?> values;
  Map<String, Object?> toJson();
}
```

## AresPdfSignatureInfo

Public model alias of `PortablePdfSignatureInfo`. The shape below documents the alias.

Integrity applies to the signed byte ranges. A valid earlier revision can
have [coversWholeDocument] false. Trust, timestamps, certification policy and
revocation are separate decisions; this is not an overall PAdES verdict.

The signed flag means a field value dictionary is present. A prepared external-signature placeholder can therefore have signed == true while cryptographicallyValid == false. Check cryptographic validity, byte-range coverage and explicit trust separately; signed alone is never a validity or trust verdict.

```dart
class AresPdfSignatureInfo {
  const AresPdfSignatureInfo({
    required this.fieldName,
    required this.signed,
    required this.status,
    required this.byteRangeValid,
    required this.cryptographicallyValid,
    required this.coversWholeDocument,
    required this.trustStatus,
    required this.signers,
    this.timestamps = const [],
    this.timestampValidated = false,
    this.revocationStatus = AresPdfRevocationStatus.notEvaluated,
    this.subFilter,
    this.error,
  });
  factory AresPdfSignatureInfo.fromJson(Map<String, dynamic> value);
  final String fieldName, status, trustStatus;
  final AresPdfRevocationStatus revocationStatus;
  /// The signature field has a value dictionary, including an unfilled external
  /// preparation. This alone does not imply cryptographic validity or trust.
  final bool signed, byteRangeValid, cryptographicallyValid, coversWholeDocument;
  final String? subFilter, error;
  final List<AresPdfSignerCertificate> signers;
  final List<AresPdfSignatureTimestamp> timestamps;
  /// True only when PDF integrity and all its CMS signer timestamps validate.
  final bool timestampValidated;
}
```

## AresPdfSignatureReport

Public model alias of `PortablePdfSignatureReport`. The shape below documents the alias.

Offline CMS report. False validation flags explicitly identify checks this
backend has not performed, even when a cryptographic signature is valid.
[timestampValidated] is true only for a nonempty set of signed fields whose
PDF integrity and every CMS signer's RFC3161 token validate with TSA roots.
It never implies current whole-file coverage, revocation, LTV or PAdES.

```dart
class AresPdfSignatureReport {
  const AresPdfSignatureReport(
    this.signatures, {
    required this.revocationChecked,
    required this.timestampValidated,
    required this.certificationPolicyValidated,
    this.revocationComplete = false,
    this.revocationValid = false,
  });
  factory AresPdfSignatureReport.fromJson(
    Map<String, dynamic> value,
  );
  final List<AresPdfSignatureInfo> signatures;
  /// Every signed field has a definitive good/revoked result for all non-anchor
  /// certificates. Missing, stale, unsupported or invalid evidence is incomplete.
  final bool revocationComplete;
  /// Every signed field has good revocation evidence. This does not imply CMS
  /// integrity, explicit chain trust, whole-file coverage, timestamp trust or LTV.
  final bool revocationValid;
  final bool revocationChecked, timestampValidated, certificationPolicyValidated;
}
```

## AresPdfSignatureTimestamp

Public model alias of `PortablePdfSignatureTimestamp`. The shape below documents the alias.

Offline RFC3161 signature-time-stamp result for one CMS signer. Validation
uses the token's authenticated generation time and explicit TSA roots. It
does not establish revocation/LTV or change ordinary PDF signer trust.

```dart
class AresPdfSignatureTimestamp {
  const AresPdfSignatureTimestamp({
    required this.signerIndex,
    required this.status,
    required this.validated,
    required this.imprintMatches,
    required this.cryptographicallyValid,
    required this.trustStatus,
    required this.timeValid,
    this.generationTime,
    this.tsa,
    this.error,
  });
  factory AresPdfSignatureTimestamp.fromJson(Map<String, dynamic> value);
  final int signerIndex;
  /// absent, notEvaluated, invalid or valid. See individual checks for policy.
  final String status, trustStatus;
  final bool validated, imprintMatches, cryptographicallyValid, timeValid;
  final DateTime? generationTime;
  final AresPdfSignerCertificate? tsa;
  final String? error;
}
```

## AresPdfSignatureVerificationOptions

Public model alias of `PortablePdfSignatureVerificationOptions`. The shape below documents the alias.

Explicit offline trust anchors. Without anchors, signature integrity is
checked but certificate trust remains notEvaluated. No OS/network roots,
revocation fetches or timestamp services are consulted. [verificationTime]
selects certificate-chain validation time, not a trusted signing timestamp.

```dart
class AresPdfSignatureVerificationOptions {
  const AresPdfSignatureVerificationOptions({
    this.trustAnchorsPem = '',
    this.timestampTrustAnchorsPem = '',
    this.crlsPem = '',
    this.ocspResponsesPem = '',
    this.requireRevocation = false,
    this.verificationTime,
  });
  final String trustAnchorsPem;
  /// Explicit TSA roots, independent of PDF signer roots. Empty means timestamp
  /// trust is not evaluated. No system roots, network TSA or revocation fetches.
  final String timestampTrustAnchorsPem;
  /// Concatenated public complete direct PEM CRLs, at most 1 MiB/64 blocks.
  /// Requires explicit signer roots and the compiled offlineCrlRevocation flag.
  /// No network retrieval, system trust changes or private keys are involved.
  final String crlsPem;
  /// Public offline OCSP RESPONSE PEM blocks, at most 1 MiB/64 responses.
  /// Requires explicit signer roots and the compiled offlineOcspRevocation profile.
  /// No responder lookup, system roots or network calls occur. The bounded
  /// profile covers issuer and authorized delegated responders, checking
  /// responder revocation unless its issuer authorizes no-check. Complete
  /// direct CRLs may cover missing per-certificate OCSP evidence; revoked
  /// wins over good, while invalid evidence never becomes a good verdict.
  final String ocspResponsesPem;
  /// Require supplied evidence; an empty evidence set rejects explicitly.
  /// Supplying either CRLs or OCSP evaluates revocation even when false.
  /// Missing chain coverage never yields a good revocation verdict.
  final bool requireRevocation;
  bool get checksRevocation;
  final DateTime? verificationTime;
  Map<String, Object> toJson();
}
```

## AresPdfSignerCertificate

Public model alias of `PortablePdfSignerCertificate`. The shape below documents the alias.

Public signer certificate details. [trusted] is null when not evaluated.

```dart
class AresPdfSignerCertificate {
  const AresPdfSignerCertificate({
    required this.subject,
    required this.issuer,
    required this.notBefore,
    required this.notAfter,
    required this.sha256,
    this.trusted,
    this.trustError,
    this.revocationStatus = AresPdfRevocationStatus.notEvaluated,
    this.revocationError,
  });
  factory AresPdfSignerCertificate.fromJson(Map<String, dynamic> value);
  final String subject, issuer, notBefore, notAfter, sha256;
  final bool? trusted;
  final String? trustError;
  final AresPdfRevocationStatus revocationStatus;
  final String? revocationError;
}
```

## AresPdfSigningIdentity

Public model alias of `PortablePdfSigningIdentity`. The shape below documents the alias.

A caller-supplied PKCS#12 identity. Keep its password in host secure storage;
the compiled signer parses copied bytes in memory and never persists keys.
Do not log this object's fields or embed a signing private key in client code.

```dart
class AresPdfSigningIdentity {
  const AresPdfSigningIdentity(this.pkcs12, {required this.password});
  final Uint8List pkcs12;
  final String password;
}
```

## AresPdfSigningOptions

Public model alias of `PortablePdfSigningOptions`. The shape below documents the alias.

Detached SHA-256 CMS signing, optionally with a CAdES signing-certificate
binding. Reuses an existing empty signature field by exact qualified name,
or creates a new invisible flat field. Set [incremental] to append an approval
while preserving prior signed bytes. Timestamps, DocMDP and LTV are separate.

```dart
class AresPdfSigningOptions {
  const AresPdfSigningOptions({
    this.fieldName = 'AresSignature',
    this.cades = false,
    this.incremental = false,
    this.signerName = '',
    this.reason = '',
    this.location = '',
    this.contact = '',
  });
  final String fieldName, signerName, reason, location, contact;
  final bool cades;
  /// Append without rewriting existing bytes. Requires the compiled
  /// `incrementalSignatures` capability. Earlier signatures must be valid and
  /// the latest must cover the input exactly; certification policies are
  /// rejected. Choose a new field name or an existing unsigned signature field.
  final bool incremental;
  Map<String, Object> toJson();
}
```

## AresPdfSize

Public model alias of `PortablePdfSize`. The shape below documents the alias.

Size in PDF points (72 points per inch), after page rotation.

```dart
class AresPdfSize {
  const AresPdfSize(this.width, this.height);
  final double width;
  final double height;
}
```

## AresPdfTable

Public model alias of `PortablePdfTable`. The shape below documents the alias.

Lay out text cells in fixed-width columns, wrapping with real font metrics.

Coordinates and column widths use PDF points. Rows stay together and grow to
fit their tallest cell. [headerRows] repeat on every inserted page. A row that
cannot fit with the headers fails the entire edit unless [splitRows] is set.
With [splitRows], overheight body rows split at measured line boundaries;
headers repeat and ordinary rows still stay together. Empty cells are supported.
Cell merging, rich cell contents and
tagged-table accessibility are not part of this operation.

```dart
class AresPdfTable extends AresPdfEdit {
  const AresPdfTable({
    required this.page,
    required this.rows,
    required this.columns,
    required this.x,
    required this.top,
    required this.bottom,
    this.headerRows = 1,
    this.padding = 4,
    this.fontSize = 12,
    this.leading,
    this.font = 'Helvetica',
    this.fontId,
    this.align = AresPdfTextAlign.left,
    this.color = AresPdfColor.black,
    this.border = AresPdfColor.black,
    this.borderWidth = 0.5,
    this.headerFill = const AresPdfColor(225, 240, 238),
    this.cellFill,
    this.paginate = false,
    this.splitRows = false,
  });
  final int page, headerRows;
  final List<List<String>> rows;
  final List<double> columns;
  final double x, top, bottom, padding, fontSize, borderWidth;
  final double? leading;
  final String font;
  final int? fontId;
  final AresPdfTextAlign align;
  final AresPdfColor color;
  final AresPdfColor? border, headerFill, cellFill;
  final bool paginate, splitRows;
  Map<String, Object?> toJson();
}
```

## AresPdfText

Public model alias of `PortablePdfText`. The shape below documents the alias.

Draw one line using a standard PDF font or an embedded TrueType resource.

Standard fonts accept printable ASCII; embedded fonts support their Unicode
glyph repertoire. Missing glyphs fail explicitly. Complex shaping is separate.

```dart
class AresPdfText extends AresPdfEdit {
  const AresPdfText({
    required this.page,
    required this.text,
    required this.x,
    required this.y,
    this.fontSize = 12,
    this.font = 'Helvetica',
    this.fontId,
    this.color = AresPdfColor.black,
  });
  final int page;
  final String text;
  final double x, y, fontSize;
  final String font;
  /// Embedded TrueType resource returned by this document's registerFont.
  final int? fontId;
  final AresPdfColor color;
  Map<String, Object?> toJson();
}
```

## AresPdfTextAlign

Public model alias of `PortablePdfTextAlign`. The shape below documents the alias.

Horizontal alignment within a paragraph or table cell.

```dart
enum AresPdfTextAlign {left, center, right, justify}
```

## AresPdfTextRun

Public model alias of `PortablePdfTextRun`. The shape below documents the alias.

An inline styled run. Adjacent runs do not introduce implicit spaces.
Registered TrueType resources allow Unicode; base PDF fonts support ASCII.

```dart
class AresPdfTextRun {
  const AresPdfTextRun(
    this.text, {
    this.font = 'Helvetica',
    this.fontId,
    this.fontSize = 12,
    this.color = AresPdfColor.black,
  });
  final String text, font;
  final int? fontId;
  final double fontSize;
  final AresPdfColor color;
  Map<String, Object> toJson();
}
```

## AresPdfTransformObject

Public model alias of `PortablePdfTransformObject`. The shape below documents the alias.

Apply the nonsingular affine matrix [a,b,c,d,e,f] to one top-level object.
Translation uses PDF points. Indices resolve in the current batch state.

```dart
class AresPdfTransformObject extends AresPdfEdit {
  const AresPdfTransformObject({
    required this.page,
    required this.objectIndex,
    this.a = 1,
    this.b = 0,
    this.c = 0,
    this.d = 1,
    this.e = 0,
    this.f = 0,
  });
  final int page, objectIndex;
  final double a, b, c, d, e, f;
  Map<String, Object?> toJson();
}
```

## AresPdfViewer

Embeds compiled platform UI, with a native-owned document and tool palette.
onCreated reports workspace creation, not first-frame/layout readiness.

```dart
class AresPdfViewer extends StatefulWidget {
  const AresPdfViewer({
    super.key,
    required this.bytes,
    this.password,
    this.options = const AresPdfViewerOptions(),
    this.onCreated,
    this.onError,
  });
  final Uint8List bytes;
  final String? password;
  final AresPdfViewerOptions options;
  final ValueChanged<AresPdfViewerController>? onCreated;
  final ValueChanged<AresPdfException>? onError;
  State<AresPdfViewer> createState();
}
```

## AresPdfViewerBackend

Internal adapter boundary, excluded from the canonical public export list.

```dart
abstract interface class AresPdfViewerBackend {
  AresPdfViewerController get controller;
  /// Resolves after compiled workspace initialization; may require widget mount.
  Future<void> get ready;
  Widget build(BuildContext context);
}
```

## AresPdfViewerController

Typed compiled viewer surface. No public raw channel escape hatch.

```dart
abstract class AresPdfViewerController extends ChangeNotifier {
  void notifyListeners();
  void dispose();
  bool get isClosed;
  AresPdfViewerState get state;
  Future<Map<String, Object?>> capabilities();
  Future<AresPdfViewerState> refreshState();
  Future<void> configure({
    AresPdfViewerTheme? theme,
    List<AresPdfViewerTool>? tools,
    AresPdfViewerHostCallbacks? hostCallbacks,
    AresPdfSigningOptions? signingOptions,
    AresPdfSignatureVerificationOptions? signatureVerificationOptions,
    AresPdfOfficeOptions? officeOptions,
    List<AresPdfOfficeFontFamily>? officeFonts,
  });
  Future<void> goToPage(int page);
  Future<void> zoomTo(double zoom);
  Future<void> cancelPlacement();
  Future<Uint8List> exportBytes();
  /// Verify the current captured PDF without replacing it. Compiled member and
  /// cryptographic capabilities are required; the native core owns permission.
  Future<AresPdfSignatureReport> verifySignatures({
    AresPdfSignatureVerificationOptions options =
        const AresPdfSignatureVerificationOptions(),
  });
  Future<void> applyEdits(List<AresPdfEdit> edits, {int? expectedRevision});
  Future<int> registerFont(Uint8List bytes, {int? expectedRevision});
  Future<int> registerImageRgba(
    Uint8List pixels, {
    required int width,
    required int height,
    int? expectedRevision,
  });
  Future<void> undo({int? expectedRevision});
  Future<void> redo({int? expectedRevision});
  Future<void> close();
}
```

## AresPdfViewerHostCallbacks

Host IO only. Null input results cancel; PDF work remains compiled. A
configured callback never falls back to a native picker or browser download.
Do not embed document signing identities in an application or SDK archive.

```dart
class AresPdfViewerHostCallbacks {
  const AresPdfViewerHostCallbacks({
    this.onImportDocx,
    this.onImportXlsx,
    this.onImportPptx,
    this.onImportJpeg,
    this.onImportAttachment,
    this.onRequestSigningIdentity,
    this.onRequestExternalCms,
    this.onSaveAttachment,
    this.onExportPagePng,
  });
  final Future<Uint8List?> Function(AresPdfHostRequestContext)? onImportDocx, onImportXlsx, onImportPptx, onImportJpeg;
  final Future<AresPdfImportedAttachment?> Function(AresPdfHostRequestContext)? onImportAttachment;
  final Future<AresPdfSigningIdentity?> Function(AresPdfHostRequestContext)? onRequestSigningIdentity;
  final Future<Uint8List?> Function(AresPdfExternalCmsRequest)? onRequestExternalCms;
  final Future<void> Function(AresPdfAttachmentExport)? onSaveAttachment;
  final Future<void> Function(AresPdfPagePngExport)? onExportPagePng;
  List<String> get names;
}
```

## AresPdfViewerOptions

```dart
class AresPdfViewerOptions {
  const AresPdfViewerOptions({
    this.theme,
    this.tools,
    this.onExport,
    this.hostCallbacks,
    this.signingOptions,
    this.signatureVerificationOptions,
    this.officeOptions,
    this.officeFonts = const [],
    this.runtime = const AresPdfRuntimeOptions(),
  });
  final AresPdfViewerTheme? theme;
  /// Native tool configuration, never a Dart entitlement verdict.
  final List<AresPdfViewerTool>? tools;
  /// Handles the export tool without a native/browser save fallback. The
  /// compiled member must advertise commonViewerHostExport. Already delivered
  /// bytes belong to the host even when its save future outlives the viewer.
  final AresPdfExportCallback? onExport;
  final AresPdfViewerHostCallbacks? hostCallbacks;
  final AresPdfSigningOptions? signingOptions;
  final AresPdfSignatureVerificationOptions? signatureVerificationOptions;
  final AresPdfOfficeOptions? officeOptions;
  final List<AresPdfOfficeFontFamily> officeFonts;
  final AresPdfRuntimeOptions runtime;
}
```

## AresPdfViewerState

Native state is a snapshot. Creation does not imply first-frame readiness.

```dart
class AresPdfViewerState {
  AresPdfViewerState({
    required this.page,
    required this.pageCount,
    required this.zoom,
    required this.documentRevision,
    required this.layoutRevision,
    required this.placementTool,
    required this.protectedReader,
    this.canUndo = false,
    this.canRedo = false,
  });
  final int page, pageCount, documentRevision, layoutRevision;
  final double zoom;
  final String placementTool;
  final bool protectedReader;
  /// Native snapshot of executable history actions, independent of the palette.
  /// Unsupported older members report false. Operations still authorize natively.
  final bool canUndo, canRedo;
}
```

## AresPdfViewerTheme

```dart
class AresPdfViewerTheme {
  const AresPdfViewerTheme({
    this.background = 0xffe2e5e7,
    this.foreground = 0xff161b22,
    this.accent = 0xff00796b,
    this.controlBackground,
    this.controlBorder,
    this.disabledForeground,
    this.focusedForeground,
  });
  final int background, foreground, accent;
  final int? controlBackground, controlBorder, disabledForeground, focusedForeground;
  Map<String, int?> toMap({required bool extendedColors});
}
```

## AresPdfViewerTool

```dart
enum AresPdfViewerTool {previous, next, zoomOut, zoomIn, undo, redo, placeNote, placeSquare, placeHighlight, cancelPlacement, note, annotations, search, pages, info, exportPdf, importDocx, importXlsx, importPptx, importJpeg, attachments, formFields, signatures, exportSigned, exportExternalSigned, exportPagePng, exportEncrypted, unlockEncrypted, exportOptimized}
```

