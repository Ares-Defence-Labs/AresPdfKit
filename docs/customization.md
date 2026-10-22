# Tools, theme and controller configuration

The current contract provides 29 semantic tools and seven opaque ARGB color roles.
These are the supported customization surface; arbitrary native menu layouts,
dialog replacement, typography and every internal UI component are not public
extension points.

## Choose tools

Supply `AresPdfViewerOptions.tools` explicitly for predictable behavior.
`tools: []` hides the palette; omitting it preserves platform legacy defaults,
which are not guaranteed identical. A hidden tool is not a permission boundary.

| Group | `AresPdfViewerTool` values |
| --- | --- |
| Navigation | `previous`, `next`, `zoomOut`, `zoomIn` |
| History/placement | `undo`, `redo`, `placeNote`, `placeSquare`, `placeHighlight`, `cancelPlacement` |
| Document panels | `note`, `annotations`, `search`, `pages`, `info`, `attachments`, `formFields`, `signatures` |
| Import | `importDocx`, `importXlsx`, `importPptx`, `importJpeg` |
| Export/security | `exportPdf`, `exportSigned`, `exportExternalSigned`, `exportPagePng`, `exportEncrypted`, `unlockEncrypted`, `exportOptimized` |

Missing host callbacks disable the corresponding configured workflow. Select
individual tools rather than assuming one action enables an entire legacy panel.
Older compiled members reject unsupported configured controls.

## Configure colors

```dart
const theme = AresPdfViewerTheme(
  background: 0xff101820,
  foreground: 0xfff4f7f8,
  accent: 0xff00bfa5,
  controlBackground: 0xff18252c,
  controlBorder: 0xff455a64,
  disabledForeground: 0xff90a4ae,
  focusedForeground: 0xffffffff,
);
await controller.configure(theme: theme);
```

Use opaque `0xffRRGGBB` integers. Colors apply only to the supported viewer roles,
not the contents of the PDF. Some native dialogs retain system styling. Complete
pixel/focus acceptance across every platform and dialog is unfinished.

Omitting `theme` preserves the current theme. Supplying
`const AresPdfViewerTheme()` resets defaults. Within a supplied theme, null optional
roles reset to derived defaults. Invalid configuration rejects atomically.

## Reconfiguration

`configure` accepts theme, tool selection, host callbacks, signing/verification
options, Office options and Office font bytes. Reconfiguration creates a new
configuration epoch and can cancel pending host requests. Do not reconfigure
inside an in-flight picker callback and then return its stale result.

Use native state notifications to update your application controls. Treat
capabilities, state and tool lists as immutable snapshots. Recreating the widget
is not needed for normal theme/tool changes and cannot reset the trial.
