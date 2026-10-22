# Feature coverage and remaining work

Version 2.0.0 documents the bounded feature set below. Full parity is deferred to the next release.

The current public contract has 89 documented types, 29 viewer tools and nine
typed host callbacks. Low-level API and viewer share the compiled processing
and offline entitlement authority.

## Implemented, bounded families

- Document creation/opening, basic readers/raster output and immutable edits.
- Supported page selection/reordering, graphics, text, images, paragraphs/tables
  and flow layout with explicit resource ownership.
- Supported AcroForm, annotation, metadata, bookmark, attachment and text workflows.
- Encryption/owner unlock, lossless optimization and supported linearized output.
- PKCS#12/CMS/CAdES signing, connected detached CMS, incremental approvals,
  RFC3161 timestamp phases and explicit-trust signature verification.
- Supplied offline CRL/OCSP checks, strict RSA-PSS policy, delegated-responder
  checks and separate trust/integrity/revocation verdicts.
- Bounded DOCX/XLSX/PPTX conversion; numeric XLSX evaluation plus
  ABS/SIGN/SQRT/INT/TRUNC/ROUND/ROUNDDOWN/ROUNDUP.
- Configurable protected editor, supported colors/tools, typed edits/history,
  application I/O callbacks and copied outputs.

Each family has limits in its guide. A working subset does not establish complete
behavioral parity with every benchmark API/member.

## Verified locally

Eight canonical runs cover Android emulator, iOS Simulator through SwiftPM and
CocoaPods, macOS programmatic checks, Linux debug/profile/release under local
containers/emulation and Web in the in-app browser. They retain resource/history,
signing, revocation, save-copy and genuine expiry checks. Independent saved-PDF
inspection checks output structure, text, fonts/images and signature math where
applicable. Processing-only and compiled-component checks remain distinct from
public consumer acceptance.

Windows is compiled/preflight only. Native macOS UI, positive paid activation,
broader physical devices, wider browsers and hardware/GPU coverage remain pending. Samsung and iPhone import/signing/export and genuine expiry checks passed.

## Substantial unfinished families

Full Office fidelity/general Excel calculation; advanced typography/layout;
structure-preserving merge/content editing; permanent redaction; OCR; complete
semantic extraction/accessibility/tagged PDF; broader XFA/layers/actions/media;
HTML/XPS/other conversion; standards/conformance families such as PDF/A, PDF/X,
PDF/UA and e-invoicing; broader repair/streaming; archival signatures and
certification policy; complete viewer/AresScan ecosystem workflows.

For signatures specifically, TSA-certificate revocation, DSS/VRI evidence,
PAdES LT/LTA and DocMDP/certification validation are not completed. Do not infer
them from ordinary timestamp or OCSP support.

The capability-family ledger still has 52 families, with some implementation in
41 and no implementation in 11. None is fully accepted on all six platforms.
This is not an exhaustive member inventory or a reliable effort-based percentage.
Windows runtime and macOS foreground UI remain explicitly unverified under the owner-approved v2 release limits.
