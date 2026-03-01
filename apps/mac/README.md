# `apps/mac`

macOS application layer for Radioform.


## Scope

- `RadioformApp/`: SwiftUI/AppKit menu bar application for onboarding, settings, and EQ controls.

## Runtime Model

Radioform on macOS is intentionally split into processes:

- `RadioformApp` (this directory): UI and orchestration.
- `RadioformHost` (`packages/host`): headless audio process.
- `RadioformDriver` (`packages/driver`): CoreAudio HAL plugin.

The UI and host communicate through file-based state (`~/Library/Application Support/Radioform/preset.json`).

Important lifecycle detail:

- While the app is running, UI failures are less likely to immediately disrupt host audio because the host is a separate process.
- When the app terminates normally, it performs cleanup and explicitly stops `RadioformHost`.

## Ownership Boundaries

- UI code and app-level orchestration: `apps/mac/RadioformApp`.
- Real-time audio processing: `packages/dsp` and `packages/host`.
- Driver install/runtime behavior: `packages/driver`.

## Related Docs

- App developer runbook: `apps/mac/RadioformApp/README.md`
- Host internals: `packages/host/README.md`
- Driver internals and install flow: `packages/driver/README.md`
