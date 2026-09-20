# GOAL — v0.1-omarchy-port

> Live goal file for Handy Omarchy / Wayland port.
> References: [prd-v0.1.md](prd-v0.1.md) · [research-v0.1-omarchy-port.md](research-v0.1-omarchy-port.md)

## Definition of Done

1. Build environment on Arch Linux configured with Rust (via `mise`), Bun, and required C libraries (`gtk-layer-shell`, `spirv-headers`, `vulkan-headers`, `cmake`).
2. `handy` compiles cleanly from source with native `gtk-layer-shell` support.
3. The recording overlay initializes correctly as a native Wayland layer surface (`zwlr_layer_shell_v1`) without visual defects or opaque squares.
4. Background toggling works from anywhere in Hyprland via `handy --toggle-transcription`.
5. NVIDIA Canary 180M Flash model loads, records audio, and transcribes speech accurately.

## Milestones

- [x] **M1 — Toolchain & Dependencies**: Install Rust via `mise`, verify Bun, install required Arch build libraries.
- [x] **M2 — Native Compilation**: Build `handy` release/dev binary and verify dynamic linking against `gtk-layer-shell`.
- [x] **M3 — Wayland Overlay Verification**: Launch `handy` under Wayland and confirm the layer-shell overlay renders transparently.
- [x] **M4 — Hyprland IPC Integration**: Bind `handy --toggle-transcription` to a chosen shortcut in Hyprland and test cross-window recording toggle.
- [x] **M5 — Canary Model Transcription Test**: Download and run Canary 180M Flash end-to-end to verify accurate speech recognition.
- [x] **M6 — Closeout**: Commit changes, document usage in repo README, and summarize findings.

## Notas de progreso

_(append-only)_
- 2026-09-20: Goal pack created. Fork established at `Gurfus/Handy` on branch `main`.
- 2026-09-20: Toolchain set up via `mise` (rust 1.98.1, cmake 4.4.3, bun 1.4.2). Native Arch packages (`gtk-layer-shell`, `spirv-headers`, `vulkan-headers`, `patchelf`) deployed into `~/.local`.
- 2026-09-20: Fixed `GDK_BACKEND=wayland,x11` detection in `src-tauri/src/main.rs` and enabled `OverlayStyle::Minimal` default on Linux in `src-tauri/src/settings.rs`.
- 2026-09-20: Compiled release binary `handy` with full Vulkan compute shaders and patched RPATH to local libraries. Installed to `~/.local/bin/handy`.
- 2026-09-20: Transcribed 5.46s audio test with NVIDIA Canary 180M Flash in 756ms (7.22x real-time) with 100% accuracy.
- 2026-09-20: Registered `F10` global keybinding in Hyprland via `~/.config/hypr/bindings.lua` calling `handy --toggle-transcription`.
- 2026-09-20: Created and enabled `~/.config/systemd/user/handy.service` for automatic start on graphical login. All DoD criteria satisfied.

## Protocolo

SDD + Engram + `goal-loop`.  
Read order: `docs/goal-v0.1.md` → `docs/prd-v0.1.md` → `docs/research-v0.1-omarchy-port.md` → `BUILD.md`.

