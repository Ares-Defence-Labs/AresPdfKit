# Viewer host callbacks and I/O

Your app selects files and saves outputs. Compiled SDK code performs conversion,
editing, rendering and signing. No callback decides licence validity.

| Callback | Input/result | Tool |
| --- | --- | --- |
| `onImportDocx` | Context → DOCX bytes or null | `importDocx` |
| `onImportXlsx` | Context → XLSX bytes or null | `importXlsx` |
| `onImportPptx` | Context → PPTX bytes or null | `importPptx` |
| `onImportJpeg` | Context → JPEG bytes or null | `importJpeg` |
| `onImportAttachment` | Context → `AresPdfImportedAttachment` or null | `attachments` |
| `onRequestSigningIdentity` | Context → `AresPdfSigningIdentity` or null | `exportSigned` |
| `onRequestExternalCms` | `AresPdfExternalCmsRequest` → detached CMS DER or null | `exportExternalSigned` |
| `onSaveAttachment` | `AresPdfAttachmentExport` → save completion | `attachments` |
| `onExportPagePng` | `AresPdfPagePngExport` → save completion | `exportPagePng` |

`AresPdfViewerOptions.onExport` separately receives copied PDF bytes for normal
and signed-copy exports. Configure it for copied signing controls.

```dart
final options = AresPdfViewerOptions(
  tools: const [
    AresPdfViewerTool.importXlsx,
    AresPdfViewerTool.exportPdf,
    AresPdfViewerTool.exportPagePng,
  ],
  onExport: savePdfBytes,
  hostCallbacks: AresPdfViewerHostCallbacks(
    onImportXlsx: (context) => chooseWorkbookBytes(context.cancelled),
    onExportPagePng: (output) => savePngBytes(output.bytes),
  ),
);
```

The picker/save functions above belong to your application. Return null to cancel
input; a configured workflow does not fall back to another native picker or
browser download. Supply Office fonts/options explicitly for workbook conversion.

## Context and cancellation

Each request carries `requestId`, `configurationId`, `documentRevision` and a
`cancelled` future. Observe cancellation to dismiss your picker/cancel your
network request. These identifiers correlate a request; they are not credentials.

Replacement, close, reconfiguration, expiry and the native 90-second request
deadline invalidate pending input. Native code checks context/revision/licence
again before accepting a result. Stale or repeated responses cannot apply to a
new request. Provider failures become `PDF_VIEWER_HOST_CALLBACK_FAILED`; private
provider exception text does not cross the bridge.

Reconfiguration is serialized. Omitted fields preserve committed options;
`const AresPdfViewerHostCallbacks()` clears callbacks. Default options reset
static configuration; an empty Office-font list clears those fonts. Do not
change configuration from inside a callback whose response you intend to return.

Once an authorized output callback starts, your application owns its byte copy
and its storage errors, even if the viewer closes before saving finishes.
Attachment names are display suggestions; sanitize them for any chosen filesystem.

## Connected signing

Return a PKCS#12 identity to let the SDK sign, or return detached CMS from your
connected signer. `AresPdfExternalCmsRequest.contentToSign` is exact PDF byte-range
content, not a digest or signed-attributes DER set. A bare RSA/ECDSA signature is
insufficient. The viewer retains its prepared PDF and validates completion.
No mutable prepared PDF crosses the connected-signer viewer boundary.

Copied signing exports a copy and preserves the live document, history, revision
and resources. Import replaces the working document with a new resource bank;
undo restores the original bank. See [signatures](signatures.md).

## Common viewer limits

| Input/output | Limit |
| --- | ---: |
| Office/JPEG import | 64 MiB |
| Attachment input/output | 64 MiB |
| PKCS#12 identity | 4 MiB |
| Detached CMS | 32,768 bytes |
| Font face | 16 MiB |
| Office fonts | 64 MiB total, at most 16 families |
| External CMS content | 64 MiB |
| Page PNG | 128 MiB |
| Copied viewer PDF export | 32 MiB |

Read actual `commonIoLimits` capabilities when needed. These bounds differ from
raw low-level engine quotas and do not promise total heap/GPU memory use.
