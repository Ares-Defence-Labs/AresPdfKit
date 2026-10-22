# Offline document certificate revocation with OCSP

This local increment adds supplied OCSP responses to document signature verification in the low-level API and protected viewer. It extends the existing offline CRL verifier. It is not a published SDK release or a claim of full PDF, Office or signature parity.

## Document trust and SDK licensing

These checks concern the certificates that signed a PDF. They do not revoke or activate an Ares PDF SDK licence. The SDK licence verifier, embedded production public key, application identity checks and genuine trial clock are unchanged. No network request is made by the OCSP verifier or by offline SDK licence verification.

PDF signature verification reports three separate results: CMS cryptographic integrity, certificate trust against the supplied roots, and certificate revocation. A signature can have valid CMS mathematics and a trusted chain while its certificate is revoked. Inspect all required results before accepting a document.

## Supply public evidence

Obtain OCSP responses through your own application or backend, independently of this SDK. Supply complete DER OCSPResponse values wrapped as public PEM blocks:

```text
-----BEGIN OCSP RESPONSE-----
<base64 of DER OCSPResponse>
-----END OCSP RESPONSE-----
```

Concatenate blocks when several responses are needed. Supply signer trust anchors in `trustAnchorsPem`. Trust is explicit: the verifier does not consult the operating system trust store or fetch issuers, CRLs, OCSP responses or responder certificates from URLs. Your application is responsible for refreshing evidence. Never embed private signing keys in evidence or pass SDK licence issuer material as document trust anchors.

## Verify through the low-level API

Use the local canonical `ares_pdf` package and `package:ares_pdf/ares_pdf.dart`. Keep its coordinated runtime dependencies together as described in [installation](getting-started.md).

```dart
import 'dart:typed_data';
import 'package:ares_pdf/ares_pdf.dart';

Future<bool> acceptSignedPdf(
  Uint8List pdfBytes, {
  required String rootsPem,
  required String ocspPem,
  String crlsPem = '',
}) async {
  final engine = await AresPdfApi.connect();
  try {
    final report = await engine.verifySignatures(
      pdfBytes,
      options: AresPdfSignatureVerificationOptions(
        trustAnchorsPem: rootsPem,
        ocspResponsesPem: ocspPem,
        crlsPem: crlsPem,
        requireRevocation: true,
      ),
    );
    return report.signatures.isNotEmpty &&
        report.signatures.every((signature) =>
          signature.cryptographicallyValid &&
          signature.trustStatus == 'trusted') &&
        report.revocationComplete && report.revocationValid;
  } finally {
    await engine.close();
  }
}
```

An empty signature list must not be accepted merely because an aggregate boolean is true. This example implements an application policy; it does not establish regulatory or legal validity.

The options constructor defaults `ocspResponsesPem` to an empty string. Empty evidence is omitted from the wire representation, preserving prior default requests. Nonempty evidence requires compiled `offlineOcspRevocation` support. Older binaries explicitly reject the request with `PDF_CAPABILITY_UNAVAILABLE`; the adapters do not silently discard evidence. Malformed option types or oversized inputs reject with the existing options error. Typed string options cannot express an array in place of PEM; raw transport rejection is tested separately.

## Verify the current viewer document

Pass the same options directly to the viewer controller:

```dart
final report = await controller.verifySignatures(
  options: AresPdfSignatureVerificationOptions(
    trustAnchorsPem: rootsPem,
    ocspResponsesPem: ocspPem,
    requireRevocation: true,
  ),
);
```

This verifies the current signed bytes without exporting and reopening a replacement PDF. Verification preserves the document revision, undo/redo history and registered font/image resources. Subsequent edits can invalidate existing signatures; successful verification before an edit does not certify the edited output.

The viewer additionally requires compiled `commonViewerOcspVerification` support. This transport capability ensures its private configuration mapper understands the evidence field, even if an older underlying Core already understands OCSP.

## Configure the signature report workflow

Set initial options on `AresPdfViewerOptions.signatureVerificationOptions`, or replace them through `controller.configure(signatureVerificationOptions: options)`. The protected Digital signatures workflow uses these settings. Configuring settings alone does not perform a check or imply a successful result. Call controller verification or open the report workflow to obtain a verdict.

```dart
final verification = AresPdfSignatureVerificationOptions(
  trustAnchorsPem: rootsPem,
  ocspResponsesPem: ocspPem,
  crlsPem: crlsPem,
  requireRevocation: true,
);

// When constructing your viewer:
final options = AresPdfViewerOptions(
  signatureVerificationOptions: verification,
);

// For an existing controller:
await controller.configure(signatureVerificationOptions: verification);
```

Web configuration checks its generation and configuration revision after the asynchronous capability query. A late older configuration cannot overwrite a newer configuration, replacement document or closed viewer. Callback configuration remains atomic.

## Freshness, chains and delegated responders

Verification evaluates signer-chain revocation for non-anchor certificates. A trust anchor is not required to revoke itself. Evidence must identify the actual certificate and issuer; an unrelated response cannot satisfy chain coverage.

Freshness checks use `thisUpdate`, `nextUpdate` and `producedAt` at the document verification time. `verificationTime` selects a document policy time; it is not an SDK licence clock override. Supplying an old time is not an archival validation or long-term-validation implementation. Applications that need current acceptance should use current, refreshed evidence and their chosen trustworthy time policy.

A response may be signed directly by the certificate issuer or by a delegated responder authorized by that actual issuer, with OCSP signing EKU and a valid signature and chain. A delegated responder must have the supported issuer-signed no-check certificate extension or independently supplied valid responder revocation evidence. Responder evidence may use OCSP or CRLs. Missing, stale or revoked responder status cannot establish a good signer result. Recursive responder checks are bounded to four levels and reject cycles.

## Mixed CRL and OCSP evidence

`crlsPem` and `ocspResponsesPem` may be supplied together. Fresh good evidence can satisfy a certificate for which the other source is missing or stale. Authenticated revocation takes precedence over good evidence. Invalid matching evidence fails closed. A malformed or invalid OCSP response can be rejected before evidence merging; do not assume every malformed mixture produces the same reported status as a well-formed revoked response.

The report retains the established statuses `good`, `revoked`, `stale`, `unknown`, `invalid` and `notEvaluated`. `revocationComplete` indicates conclusive coverage, including a revoked result. `revocationValid` indicates a good result. Do not equate completeness with acceptance.

## Supported policy and bounds

Only successful BasicOCSPResponse envelopes are processed. DER must be fully consumed and canonically re-encode. Unknown critical extensions, weak signature algorithms and malformed algorithm parameters are rejected. SHA-1 is supported for certificate identifiers only, not as an accepted response-signature digest. Identifier hashes support SHA-1, SHA-256, SHA-384 and SHA-512. Response signature validation uses the strict supported SHA-2/RSA-PSS policy also applied to CRLs.

The public evidence string is bounded to 1 MiB, at most 64 responses, and at most 128 SingleResponse entries per response. Responder recursion is bounded to four. Other existing certificate, chain, document and transport resource limits still apply. No error, success response or diagnostic should be treated as permission to bypass these limits.

## Local verification evidence

The current local SDK has separate API and current-viewer acceptance on Android,
iOS through SwiftPM/CocoaPods, macOS programmatically, Linux debug/profile/release
and Web. Each run retains the supplied OCSP/CRL/RSA-PSS cases, resource/history
preservation and genuine expiry checks. Windows runtime and macOS foreground UI
remain pending. This does not qualify TSA-certificate revocation, archival
DSS/VRI/PAdES LT/LTA or certification policy. See [feature status](feature-status.md).

