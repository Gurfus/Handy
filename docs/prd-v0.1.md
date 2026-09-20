# PRD — v0.1-omarchy-port

> Input for the SDD and implementation loop.
> Read: [goal-v0.1.md](goal-v0.1.md) → this → [research-v0.1-omarchy-port.md](research-v0.1-omarchy-port.md).
> Status: **ready-for-agent** · 2026-09-20

## Problem Statement

Handy provides state-of-the-art local transcription (including NVIDIA Canary 180M Flash and Whisper models), but its prebuilt Linux package is non-functional for daily use on modern Wayland/Hyprland environments like Omarchy. Specifically:
1. Keyboard shortcuts cannot be triggered when Handy is in the background.
2. The recording status overlay renders as an unsightly broken rectangle due to missing Wayland layer-shell protocol initialization.
3. Building from source on Arch Linux requires clear dependency resolution and toolchain alignment.

## Solution

Deliver a production-ready build of Handy tailored for Omarchy / Arch Linux:
- Compile natively against `gtk-layer-shell` to ensure the recording pill floats with proper alpha compositing above Hyprland windows.
- Provide seamless toggle control via `handy --toggle-transcription`, bound natively in Hyprland's input configuration.
- Support smooth execution of NVIDIA Canary 180M Flash via ONNX Runtime without audio clipping.

## User Stories

1. **As a developer using Omarchy / Hyprland**, I want to press a global keybinding (e.g., F10) from any application or workspace, so that Handy toggles recording without needing focus.
2. **As a user speaking into the microphone**, I want to see a sleek, transparent, non-blocking recording pill showing microphone activity, so that I know my speech is being recorded.
3. **As a user finishing dictation**, I want Canary 180M Flash to transcribe my speech fast and accurately, pasting the resulting text directly into my active window.

## Implementation Decisions

- **Environment Flags**: Enforce `GDK_BACKEND=wayland,x11` during execution to guarantee GDK requests Wayland display connections when available.
- **IPC Protocol**: Leverage Tauri's `tauri-plugin-single-instance` handler for `--toggle-transcription` and `--toggle-post-process`.
- **Packaging / Execution**: Produce a clean native binary (`handy`) and a helper script or systemd user service for background execution.

## Testing Decisions

- **Unit / Build**: Compile `handy` with Cargo and verify linking of `libgtk-layer-shell.so`.
- **Manual Verification**: Run `handy` on Hyprland, trigger `--toggle-transcription`, verify the layer-shell overlay appears without visual corruption, and verify transcription output with Canary 180M Flash.

## Out of Scope

- Rewriting the Tauri front-end or modifying upstream settings schemas.
- Modifying the existing system-wide Voxtype installation (which remains untouched and available as fallback).
