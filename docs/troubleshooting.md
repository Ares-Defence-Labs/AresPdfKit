# Errors and troubleshooting

Catch `AresPdfException`, inspect `code`, and use copied `details` when supplied.
Do not treat provider exception strings as a trusted SDK entitlement result.
Native operation errors should leave the previous readable document intact.

| Code/category | Meaning and response |
| --- | --- |
| `PDF_LICENCE_REQUIRED` | Advanced operation denied by native licensing; ship a valid portal key. Reopening an owner does not reset trial. |
| `PDF_CAPABILITY_UNAVAILABLE` | Binary lacks the requested profile; install a coherent supported SDK build. |
| `PDF_STALE_REVISION` | Document changed before command admission; refresh state and decide whether retry is valid. |
| `PDF_HISTORY_EMPTY` | No matching undo/redo action exists; use native state availability. |
| `PDF_PAGE_RANGE` | Invalid/duplicate page selection or out-of-range page. |
| `PDF_UNLOCK_REQUIRED` | Protected input needs the supported owner-password path. |
| `PDF_SIGNED_REWRITE_REQUIRED` | Rewriting/optimization would affect signed/prepared content; use a supported workflow and preserve original bytes. |
| `PDF_VIEWER_HOST_CALLBACK_FAILED` | Application I/O provider failed; handle its original error privately in the provider. |
| `PDF_VIEWER_HOST_BYTES_SIZE` | Callback data exceeds the common bound; reduce input or choose an appropriate low-level path. |
| `PDF_VIEWER_THEME_UNSUPPORTED` | Unsupported theme role/profile; check actual compiled capabilities. |
| `PDF_XLSX_FORMULA_*` | Explicit calculation grammar/policy/domain/limit failure; see spreadsheet guide. |

This table is not the complete dynamic error-code universe. The API reference
and each workflow guide define more specific policies.

## Common integration problems

- **No viewer:** give the widget bounded layout, await initialization and inspect
  `onError`. Creation is not first-frame presentation.
- **Tool disabled:** select the tool and supply its matching callback/options.
- **Picker result ignored:** it may be cancelled, timed out, stale or from a
  retired configuration/document. Do not replay the result into a new request.
- **Wrong font/image handle:** IDs belong to the registering document lineage.
  Imports create a new bank; undo restores the original.
- **Empty text:** embedded text extraction is not OCR.
- **Office conversion rejects:** inspect the typed fidelity report and required
  fonts; a complete general Office engine is not yet implemented.
- **A signed file looks fine but fails trust/revocation:** rendering, CMS math,
  explicit chain trust, timestamps and revocation are separate checks.
- **Native plugin missing:** rebuild the host after `flutter pub get`; hot reload
  does not install native frameworks/plugins.
- **Web works locally but fails on host:** verify Wasm MIME, complete asset paths,
  origin/isolation/CSP headers and version-consistent cache contents.

## Report a reproducible issue

Include SDK/toolchain version, platform/OS/device/browser, build mode, entry point,
error code, minimal reproduction and expected behavior. Provide only synthetic
or sanitized documents that you are authorized to share. Never attach real keys,
passwords, PKCS#12 identities, private PDFs or account tokens. See [support](../SUPPORT.md).
