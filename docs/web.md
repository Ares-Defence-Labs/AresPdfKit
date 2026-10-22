# Web runtime and hosting

The Web API and viewer use compiled WebAssembly and packaged assets. The public
factory selects the platform; developers do not supply a native library path.

The default runtime root is:

```text
assets/packages/ares_pdf_protected_web/assets/runtime/
```

Keep the complete distribution together on the same origin, including runtime
bootstrap, Flutter resources, fonts and compiled Core/PDF renderer. No private
viewer Dart, source maps or JavaScript viewer fallback belong in the customer
distribution. Do not add a CDN rewrite that silently serves different binaries.

## Hosting headers

Use a secure production origin and serve `.wasm` as `application/wasm`. The local
qualified host uses:

```http
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
Cross-Origin-Resource-Policy: same-origin
```

These headers affect all embedded resources on the page. Ensure your own fonts,
images and third-party integrations satisfy the browser's isolation policy.
Check your browser's requirements rather than disabling isolation to hide failures.

CSP must permit the packaged runtime, WebAssembly compilation, required blob
workers and its packaged resources. The exact policy depends on your app and
compiled Flutter version; the test host's permissive development policy is not
a production CSP template. No arbitrary network origin is required by offline
licence checks or supplied document CRL/OCSP verification.

## Custom root

`AresPdfRuntimeOptions(webRuntimeRoot: ...)` selects the same-origin asset root
when your app hosts the exact runtime elsewhere. Use the same options for API
and viewer. Changing the root does not select an entitlement verifier or replace
compiled feature support. A missing notice/viewer asset is an initialization
failure, not an API-only way to evade trial policy.

## Browser lifecycle

Await API/controller calls. Keep viewer constraints finite. Closing a viewer
drains its document/workspace; the page-lifetime notice authority can remain.
Do not use iframe reload, another API owner or widget recreation to assume a new
trial. Already delivered byte copies belong to your application; use your own
download/storage logic.

## Troubleshooting

Inspect actual network responses for missing assets, HTML returned as Wasm,
wrong MIME types, CSP/COEP failures and mismatched runtime versions. Test a fresh
build with a coherent asset cache. A browser pass on one engine does not qualify
all Safari/Firefox/Chromium versions or mobile Web memory/GPU behavior.
