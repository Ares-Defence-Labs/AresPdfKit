# Attachments, metadata, bookmarks and search

| Task | Entry points |
| --- | --- |
| Document information | `documentInfo()`, `metadata()` |
| Metadata changes | `setMetadata(...)`, `AresPdfSetMetadata` |
| Bookmark changes | `AresPdfSetBookmarks`, `AresPdfBookmark` and destination DTOs |
| Embedded files | `attach`, `attachments`, `attachmentBytes`, `AresPdfDeleteAttachment` |
| Embedded text | `extractText(page)`, `search(query, ...)` |

Read the full signatures and defaults in [API reference](api-reference.md).

## Embedded files

`attach(name, bytes)` returns an independently owned snapshot. Listing/extraction
uses the supported attachment index/order contract. Treat an attachment's name
as untrusted display metadata, never as a path to write directly. Your app
chooses safe names, destinations and whether content may be opened.

The viewer uses `onImportAttachment` and `onSaveAttachment`. Returning null
cancels import. A configured callback does not fall back to another picker.
Portfolio behavior, every embedded-file relationship and rich-media lifecycle
are separate, incomplete feature families.

## Metadata and bookmarks

Apply metadata through the returned snapshot or typed edit. Null values remove
supported entries according to the DTO contract. Do not infer complete XMP,
color-space, archival metadata or tagged-structure support from ordinary string
metadata access.

Bookmark destination coordinates and one-based pages must be valid for the
document. Validate saved navigation when reordering/merging documents; general
cross-document destination and structure preservation remains incomplete.

## Text extraction and search

Search operates on supported embedded PDF text, with optional case-sensitive
and whole-word behavior. It is not OCR. An image-only scan can legitimately
return no text. General table/semantic structure extraction, complex-script
ordering, full text accessibility and selectable text layers remain unfinished.
Do not treat extracted text as untrusted instructions to execute.

All inspection/edit/export methods retain their native licence, ownership and
document-permission policy. See [low-level lifecycle](low-level-api.md).
