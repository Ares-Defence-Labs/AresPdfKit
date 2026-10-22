# Embed the protected viewer/editor

`AresPdfViewer(bytes: pdfBytes, ...)` accepts copied document bytes and embeds the
compiled platform workspace. Give it finite, nonzero layout constraints, such as
`Expanded` within a `Column`. Keep an enclosing Material/Directionality host for
your app's navigation and controls.

See the README for a minimal widget. A complete host integration is in
[host callbacks](host-callbacks.md). The SDK does not choose your application's
storage path, upload destination or account identity.

## Creation and lifetime

`onCreated` means compiled workspace initialization completed. It does **not**
guarantee the first rendered frame or final widget layout is visible. Use the
provided controller only while that workspace remains alive. Widget replacement,
removal and late asynchronous creation follow the close barrier.

Subscribe with `addListener` for `state` snapshots and remove application listeners
when your app stops observing the viewer. `state` exposes page, page count, zoom,
document/layout revisions, placement tool, protected-reader status and native
`canUndo`/`canRedo`. Listener notification is not a writable document.

The widget manages workspace lifetime. `controller.close()` is awaitable when
explicit coordination is needed; do not start new work after closing it. Repeated
close is safe and never resets the compiled trial.

## Commands

```dart
await controller.goToPage(2); // One-based.
await controller.zoomTo(1.25);
await controller.cancelPlacement();
final bytes = await controller.exportBytes();
```

Commands are asynchronous and may reject for unavailable features, stale state,
closed owners, PDF permission restrictions or expired licensing. Save the returned
bytes through application-owned I/O. Exporting is a protected operation even if
your toolbar is hidden.

## Edit the current document

```dart
final revision = controller.state.documentRevision;
await controller.applyEdits([
  const AresPdfText(page: 1, text: 'Reviewed', x: 36, y: 72),
], expectedRevision: revision);
await controller.undo();
await controller.redo();
```

The optional revision is checked inside the compiled owner. `PDF_STALE_REVISION`
means the document changed before execution; refresh state and decide whether to
retry. Do not blindly replay non-idempotent edits. Layout revision is separate.
An empty history rejects with `PDF_HISTORY_EMPTY`.

Register fonts/images on this controller before referring to their IDs. Typed
edits preserve the native resource bank through supported history snapshots.
Office/JPEG imports create a new bank; undo restores the original document/bank.
Copied signing preserves the current live document, resource IDs and history.

## Save copies and errors

`onExport` receives an already-authorized owned copy. Your save future may outlive
the viewer because your app owns bytes already delivered. A failed application
save does not imply that the PDF operation failed; show the error and let the
user choose a new destination. Do not re-export automatically after licence expiry.

`onError` receives initialization/widget errors. Awaited controller calls still
need their own `try/catch`. Host callbacks must handle their own I/O and respect
cancellation. Signature verification before an edit does not certify edited bytes.

## Coverage

The editor is the intended integration surface for AresScan+, but migrating the
legacy app's GlobalKey/state/path workflows remains unfinished. Native macOS
foreground menus/gestures/export acceptance, Windows runtime, full text selection,
accessibility and complete viewer parity remain pending. See [feature status](feature-status.md).
