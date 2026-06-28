Usfully fle# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Packet Journey — Inside the Machine** is a cinematic, scrubbable simulation of what happens when you type a URL and press Enter. It visualizes the full path of a network request through hardware and software layers (application, kernel, TCP/IP, NIC, internet, and back).

## Key Files

- **`Packet Journey v2.dc.html`** — The main (and only) simulation. A persistent motherboard view with holographic software panels, ending in a self-contained TCP-handshake network finale. This is the file to work on.
- **`support.js`** — Auto-generated Design Component runtime. **Do not edit.** Built from `dc-runtime/src/*.ts` via `cd dc-runtime && bun run build`.

## Architecture

This is a **Design Component** (`.dc.html`) project — no build step, no external dependencies, no server. Each `.dc.html` file is a self-contained HTML document using the `<x-dc>` template format with a `Component extends DCLogic` class.

**Rendering engine:** A single master timeline drives a `requestAnimationFrame` loop. Each frame:
1. Computes camera position (smooth eased fly between focus points)
2. Projects board-space coordinates to screen space for holographic UI
3. Renders the active scene's panels, beams, and board-space elements

**Template system:** Uses `{{ expression }}` bindings, `<sc-if>` conditionals, and `<sc-for>` list iteration — all provided by the `support.js` runtime.

**Layer color coding:** HTTP=`#4fd6e0` (cyan), TCP=`#54e08a` (green), IP=`#ffb454` (amber), Ethernet=`#c084fc` (violet), IRQ=red.

## Running

```bash
open "Packet Journey v2.dc.html"   # macOS — or just double-click in file explorer
```

Best experience in a Chromium-based browser at desktop window size. Loads fonts from Google Fonts; otherwise fully offline.

## 13 Scenes (v2)

The simulation is a continuous camera fly-through: Your Desk → Living Board → Application/RAM → CPU Execution → System Call (Ring 3→0) → Kernel Networking → Memory Objects → TCP Segment → IP Routing → NIC Driver → NIC Firmware/DMA → Interrupt/IRQ → **TCP Handshake & The Journey**.

The final scene is a self-contained cinematic that leaves the motherboard behind: the board fades out, the camera follows packets over animated EM waves through the Internet (PC → router → ISP → undersea fiber → Google edge → server) and walks the full TCP lifecycle — **SYN → SYN-ACK → ACK** three-way handshake, then **segmentation** of the HTTP request into numbered TCP segments, transit, **reassembly** in sequence order at the server, and the **200 OK** reply traveling home. It renders in its own world-space coordinate system (not the board's camera projection): a wide scrollable world with a packet-following camera, a 3-way-handshake ladder, a TCP-state HUD, and a live reassembly buffer. See `scJourney()` and its helpers (`stars`, `jpkt`, `reasmBuffer`, `handshakeLadder`).