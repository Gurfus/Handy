# Prompt — v0.1-omarchy-port

> Kickoff for the Handy Omarchy / Wayland port execution.
> Docs: [goal-v0.1.md](goal-v0.1.md) · [prd-v0.1.md](prd-v0.1.md) · [research-v0.1-omarchy-port.md](research-v0.1-omarchy-port.md)

```text
GOAL: Compile and deploy Handy natively for Omarchy / Arch Linux with proper Wayland gtk-layer-shell overlay, Hyprland IPC toggle, and Canary 180M Flash model support.

REPO
- Path: /home/ormarchyserver/work/Handy
- Remote: git@github.com:Gurfus/Handy.git

═══════════════════════════════════════
MODO: GOAL + SDD
═══════════════════════════════════════

Follow Definition of Done and Milestones in docs/goal-v0.1.md.
M1: Toolchain & Dependencies
M2: Native Compilation
M3: Wayland Overlay Verification
M4: Hyprland IPC Integration
M5: Canary Model Transcription Test
M6: Closeout

═══════════════════════════════════════
DIRECCIÓN LOCKED
═══════════════════════════════════════

SÍ:
- Native gtk-layer-shell compilation.
- Hyprland keybinding dispatching handy --toggle-transcription.
- GDK_BACKEND=wayland,x11 runtime posture.
- Calibrated ALSA/PipeWire audio settings.

NO:
- No X11 grab hacks.
- No modifications breaking existing Voxtype production setup.
- No unneeded UI restyling.
```
