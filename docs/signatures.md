# Certificate signatures, connected signers and timestamps

An SDK licence string is not a document signing identity. A visual signature image
or unsigned `/Sig` field is not a cryptographic PDF signature.

## PKCS#12 signing

```dart
final signedBytes = await engine.signPdf(
  pdfBytes,
  AresPdfSigningIdentity(pkcs12Bytes, password: identityPassword),
  options: const AresPdfSigningOptions(
    fieldName: 'Approval1',
    cades: true,
    reason: 'Reviewed',
  ),
);
```

The compiled signer produces detached SHA-256 CMS with optional CAdES certificate
binding. An exact existing empty field can be used; otherwise the supported
profile creates an invisible flat signature field. Store identity passwords/bytes
securely in your application and never include private identities in public docs.

## External detached CMS

```dart
final request = await engine.prepareExternalSignature(pdfBytes);
final cms = await yourSigningService(request.contentToSign);
final signed = await engine.completeExternalSignature(request.preparedPdf, cms);
```

`yourSigningService` is app-owned. Sign exact `contentToSign`: it is PDF byte-range
content, not a precomputed digest or DER signed attributes. Return complete
detached CMS/CAdES DER, not bare signature bytes. Do not edit/re-save prepared
bytes between phases. The SDK validates completion against the retained content.

The viewer connected callback exposes only its exact content and cancellation
context; the prepared PDF stays inside the viewer. Copied signing does not mutate
the live viewer's revision/history/resource bank. Configure `onExport` for the
result and the appropriate identity or connected-signer callback.

## Incremental approvals

`AresPdfSigningOptions(incremental: true, fieldName: 'Approval2')` selects the
bounded append-only path. Earlier signatures must be valid and the latest must
cover the input exactly. Choose a new field or supported empty signature field.
Unsupported certification policies reject. General edit/rewrite operations are
not incremental signing and may invalidate earlier signatures.

## RFC3161 timestamps

The API exposes `createSignatureTimestampRequest(cms, policyOid: ...)` and
`completeSignatureTimestamp(cms, request: ..., response: ...,
timestampTrustAnchorsPem: ...)`. Your application/backend obtains the timestamp
response; the SDK does not call a TSA. Keep request/response/CMS bytes paired.
The completion path checks the request binding and explicit TSA trust under its
bounded SHA-2 profile. Token verification separately checks imprint, cryptographic
signature, generation time, purpose and explicit TSA roots.

TSA trust roots are independent of ordinary PDF signer roots. Do not infer a
trusted timestamp from a displayed signing date. TSA-certificate revocation,
DSS/VRI, PAdES LT/LTA and general archival/certification policy remain unfinished.

## Verification policy

`verifySignatures` returns separate CMS, chain trust, timestamp, byte coverage and
revocation results. A `/V` value only means a field has signature data; it does
not establish mathematical validity. Empty signature lists must not pass a
policy intended to require a signed document.

Verify before accepting a document and after any transformation you intend to
accept. Read [offline revocation](document-revocation.md) for a complete policy
example. Current SDK acceptance does not establish regulatory/legal validity or
general validation of every signature format.
