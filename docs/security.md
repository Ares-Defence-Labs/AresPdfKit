# Encryption, protected readers and optimization

## Password protection

```dart
final encrypted = await engine.encrypt(
  pdfBytes,
  AresPdfEncryption(
    ownerPassword: ownerPassword,
    userPassword: userPassword,
    allowCopy: false,
    allowModification: false,
  ),
);
```

The supported profile uses compiled PDF encryption/permission processing. Do not
log passwords or place real credentials in examples. Document permission flags
are distinct from the SDK subscription entitlement.

`openProtectedReader(bytes, password: ...)` opens a restricted reader where
supported; it does not grant an editor lease or general plaintext export.
`decrypt(bytes, ownerPassword: ...)` requires the owner-authorized unlock path.
Incorrect password, unsupported protection or insufficient permissions reject
explicitly. Ordinary viewers elsewhere may enforce PDF permissions differently.

## Optimization

`engine.optimize(bytes, linearize: ..., recompressFlate: ..., objectStreams: ...)`
returns losslessly rewritten PDF bytes under the bounded profile. It does not
guarantee a smaller file. Preserve returned bytes directly rather than opening and
re-saving them if exact linearization matters.

Optimize before signing. Signed, prepared-signature, timestamped or certified
input rejects with `PDF_SIGNED_REWRITE_REQUIRED`; encrypted input requires the
documented unlock path and can reject with `PDF_UNLOCK_REQUIRED`. Optimization
is not OCR, sanitization, conformance repair or permanent redaction.

## Viewer workflows

The `exportEncrypted`, `unlockEncrypted` and `exportOptimized` tools expose their
individual supported workflows. Copied output uses the configured host export
contract where supported; do not assume arbitrary native dialogs can be replaced
or all system styles inherit custom colors.

Both operation admission and final result delivery remain compiled/licence-gated.
Queued work is not a way to export after trial expiry. See [licensing](licensing.md)
and [host ownership](host-callbacks.md).
