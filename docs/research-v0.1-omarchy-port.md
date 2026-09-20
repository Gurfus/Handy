# Research — v0.1-omarchy-port

> Findings and technical architecture shaping the Omarchy / Wayland port of Handy.
> Date: 2026-09-20.

## Snapshot

The prebuilt upstream Handy Linux AppImage fails on Omarchy (Arch Linux + Hyprland / Wayland) in two major ways:
1. **Global Shortcuts**: Tauri's `tauri-plugin-global-shortcut` relies on X11 key grabs via XWayland. Wayland compositors intentionally isolate input between windows for security; background applications cannot intercept global keystrokes without explicit compositor IPC or the XDG Desktop Portal GlobalShortcuts protocol.
2. **Visual Overlay Glitch**: The recording overlay appears as an undecorated, non-transparent gray/black empty rectangle because the upstream generic binary lacked native `libgtk-layer-shell.so` linkage on Wayland and was forced into XWayland fallback mode without proper compositor alpha blending.
3. **Model Performance**: NVIDIA Canary 180M Flash via `transcribe-rs` (ONNX Runtime) offers exceptional accuracy and multilingual transcription, but requires clean 16 kHz audio without ALSA hardware gain clipping.

## Decisions / Locks

| Topic | Decision |
|---|---|
| Wayland Window Protocol | `gtk-layer-shell` (`zwlr_layer_shell_v1`) native surface for the recording overlay. |
| Global Key Handling | Native Hyprland keybinding dispatching `handy --toggle-transcription` via Tauri single-instance IPC. |
| Runtime Environment | Set `GDK_BACKEND=wayland,x11` and ensure `WEBKIT_DISABLE_DMABUF_RENDERER=1` to prevent GTK crashes. |
| Toolchain Management | Userland Rust (`stable`) via `mise`, avoiding unnecessary system-level root overrides. |
| Host Packages | Required Arch packages: `gtk-layer-shell`, `spirv-headers`, `vulkan-headers`, `cmake`. |
| Audio Pipeline | Preserved calibrated ALSA gain (0 dB boost, 50% capture volume) feeding 16 kHz mono. |

## Rejected

| Idea | Why |
|---|---|
| Patching `tauri-plugin-global-shortcut` with X11 grabs | Does not work on Wayland compositors; violates security boundaries and causes silent key drops. |
| Virtual X11 window fallback for overlay | Produces the broken opaque square artifact on Hyprland/Wayland without compositor anchoring. |
| Replacing Handy with a custom CLI micro-daemon | Discards Handy's established UI, audio toolkit, history, model downloader, and VAD pipeline. |

## Direction (Locked)

1. Maintain `Gurfus/Handy` fork as the canonical repository for Arch/Wayland users.
2. Install build prerequisites cleanly on Arch Linux.
3. Verify that native compilation produces a working `gtk-layer-shell` overlay surface under Hyprland.
4. Integrate Hyprland keybindings calling `handy --toggle-transcription`.
5. Ensure Canary 180M Flash runs with optimal CPU/GPU acceleration on AMD Ryzen 7 4800H + Radeon RX 5500M.

## Implications

- Backlog next: Optional XDG Desktop Portal `GlobalShortcuts` integration if pure IPC proves insufficient.
- Out of scope: Complete UI redesign of Handy's settings views. Keep changes surgical and native.
