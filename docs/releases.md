# Releases and package distribution

Version 2.0.0 covers the [documented feature set](feature-status.md). Full
Syncfusion parity is deferred to the next release. Windows runtime and macOS
foreground UI remain [explicitly unverified](platforms.md) under the owner's
version-specific approval. Unverified tests are never marked passed.

The verified publisher is **aresdefencelabs.com**. There are three coordinated
hosted packages: `ares_pdf_protected_web`, `ares_pdf_protected_desktop`, then
`ares_pdf`. Developers add only `ares_pdf` directly. Exact matching runtime
versions prevent binary/contract drift.

The public Git repository contains documentation and support material. Its
metadata pubspec is not an installable SDK. The pub.dev packages contain the
public Dart contracts, compiled native/Wasm runtimes, notices and example.
The SDK uses approved commercial terms; documentation retains separate MIT terms.

Private release assembly binds source snapshots, compiled inventories, genuine
runtime evidence, dependency notices and approved version scope. Cloudflare
validates signed GitHub webhooks and requires a separately recorded owner
approval of the immutable release SHA and exact review digest. Runner credentials
and GitHub release Apps cannot create that approval through pipeline APIs.
GitHub environment-required reviewers are unavailable on the current plan;
the independent Cloudflare approval is the enforced publication gate.

Initial package publication requires an authenticated manual bootstrap before
GitHub OIDC can be configured. Each package's company publisher ownership is
verified separately. Future automation uses the private repository, exact version
tag pattern and `pub.dev` environment, without a long-lived pub.dev token.
Registry confirmation gates public documentation/tag/release synchronization.

Never include a customer licence string, document identity, issuer private key,
source map or debug symbol in public release inputs. Never replace or retarget
an immutable published version. See [Dart publishing](https://dart.dev/tools/pub/publishing)
and [automated publishing](https://dart.dev/tools/pub/automated-publishing).

The public SDK version is **2.0.0**. Its exact runtime dependencies are Web **2.0.0** and Desktop **2.0.1**. The Desktop packaging patch preserves original macOS framework signatures and symlinks inside local ZIP artifacts; no native implementation or licence trust pin changes. Runtime component patch versions can differ from the public SDK version. Future release reviews must select unused component versions before uploading.
