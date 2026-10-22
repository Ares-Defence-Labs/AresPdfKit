# Offline SDK licensing, trial and renewal

## Developer flow

Purchase the PDF SDK through the Ares Defence Labs portal. Copy the complete
digitally signed licence string for that SDK and supply it in your app's code or
private build configuration. End users do not need to enter a portal key.

```dart
const licenceKey = String.fromEnvironment('ARES_PDF_LICENCE');
final engine = await AresPdfApi.connect();
if (licenceKey.isNotEmpty) {
  final accepted = await engine.activate(licenceKey);
  // Handle false as trial state, not as permission to bypass the native policy.
}
// Retain engine for app operations and await engine.close() on completion.
```

`--dart-define=ARES_PDF_LICENCE=...` supplies build configuration. Do not commit a
real customer string to this public repository. The deployed app necessarily
contains its embedded licence; this is a signed entitlement, not a secret API
credential or the private issuer key.

## What the device checks

Compiled code verifies the backend signature, product, applicable application
binding and expiry using its embedded public verifier key and actual platform
context. It does not trust caller booleans, app-ID overrides, a callback verdict
or a Dart-selected clock. Editing a key's dates/claims invalidates its signature.
No licensing API call is required for normal local verification.

Positive paid/owner activation and physical-device acceptance remain release
gates for the current candidate. Do not infer a production acceptance result
from public-key presence or successful trial-mode operations. Sandbox keys need
a properly sandbox-trusted test build; production builds must not substitute
their public key just to accept sandbox data.

## Trial behavior

Missing, invalid or expired keys select the genuine shared 60-second process/page
trial. API and viewer use the same compiled authority; creating another engine,
replacing widgets or reopening a document does not start another trial.

The SDK presents the exact dismissible notice:

> You are using a trial version of Ares Defence Labs PDF SDK

The recurring notice/expiry policy is native-owned. After expiry, advanced edits,
conversion, signing and protected exports reject; supported retained basic
reader/raster operations remain available. A UI dismissal is not a licence.
Read capability and error contracts instead of assuming all inspection/encoding
operations remain available after expiry.

## Twelve-month renewal

The agreed commercial workflow uses 12-month keys, developer notices at 3, 2 and
1 month before expiry, and renewal charging/key issuance 30 days before expiry.
The replacement term begins at the existing expiry. Portal/email notices and
billing are backend responsibilities, not SDK background network behavior.
This SDK documentation does not certify production billing/email configuration.

Updating a portal string does not update installed apps. Developers must deliver
the replacement key before expiry through their own app-release/configuration
strategy. Offline verification cannot instantly revoke an already issued key or
obtain authoritative online time. These properties follow from the offline model.

## Different keys, different purposes

SDK issuer private keys belong only to the licensing backend. PKCS#12 document
identities belong to your signing workflow. Document trust anchors/CRL/OCSP
responses are public verification evidence. Never interchange these materials.
See [document revocation](document-revocation.md).
