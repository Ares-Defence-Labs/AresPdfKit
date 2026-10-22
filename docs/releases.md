# Releases and package distribution

No PDF SDK version is published by this local preparation. This public repository
is documentation/support only; its metadata pubspec is not an installable SDK.
The reviewed SDK package will contain documented Dart contracts, compiled members,
required Web assets, third-party notices and a runnable example.

## Planned release flow

The private source repository prepares native/Web builds and verifies the exact
candidate. Release automation follows the existing Edge model: validated
changelog, authorized immutable version tag, private ephemeral runner, private
artifacts, pub.dev OIDC publication and public documentation synchronization only
after the registry confirms the release. The local PDF configuration is disabled.

There are three coordinated hosted members. Publish runtime dependencies first,
then the customer-facing `ares_pdf` facade, using exact matching versions and
reviewed inventories. Developers still add only `ares_pdf` directly.

Activation requires separate configuration of the private GitHub workflow,
scoped Apps/runner, Cloudflare Worker/D1/Queues/webhook/secrets and pub.dev package
automation. Writing configuration files locally does not connect remote accounts
or create those resources. No deployment or activation is performed by these docs.

## Publishing prerequisites

Final SDK licence terms, dependency redistribution review, complete requested
feature acceptance and outstanding platform/paid-activation checks must close
before the package is presented as ready. The current source's placeholder SDK
licence is not a publishable legal file. The public documentation MIT licence
does not replace the SDK terms or third-party notices.

Pub.dev currently requires an initial manual publication before automated OIDC
publishing can be configured. Each member needs its own matching package/tag
authorization. See [Dart automated publishing](https://dart.dev/tools/pub/automated-publishing)
and [publishing requirements](https://dart.dev/tools/pub/publishing).

Release preparation keeps `publish_to: none`, publication flags disabled and
private artifacts out of this public repository. No real customer licence,
document identity, issuer private key, source map or debug symbol may be exported.
Never replace or retarget a published package version/tag.
