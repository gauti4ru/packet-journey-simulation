# Packet Journey — Inside the Machine

A cinematic, scrubbable simulation of **what physically happens when you type a URL and press Enter**. The camera dives through your screen onto a *living motherboard* and follows a single web request down through every hardware and software layer — application, kernel, TCP/IP, the NIC and its firmware, out the wire, across the internet, and back as a rendered page.

Built as a single self-contained **Design Component** (`.dc.html`) — no build step, no dependencies. Open it in a browser and it runs.

---

## Files

| File | What it is |
|------|------------|
| **`Packet Journey v2.dc.html`** | **The simulation.** A persistent, always-visible motherboard with holographic software panels floating *above* the physical hardware, ending in a full cinematic TCP handshake across the Internet. This is the one to open. |
| `support.js` | The Design Component runtime (auto-generated). Required by the `.dc.html` file. Do not edit. |
| `CLAUDE.md` | Guidance for working on the project with Claude Code. |

> Open **`Packet Journey v2.dc.html`** directly in any modern browser.

---

## What you see (v2)

The motherboard **never disappears**. Software always appears as holographic overlays anchored to the hardware that's executing it, so you always know *where* on the board things are happening.

**13 scenes, one continuous camera fly-through:**

1. **Your Desk** — type a URL in a realistic browser, press Enter, camera dives through the glass.
2. **The Living Board** — a real motherboard, alive: CPU, RAM, VRM, chipset, NIC, SSD, PCIe slot, cooling fan, glowing copper traces, animated data buses, amber power rails, and clock pulses.
3. **Application · RAM** — the browser process builds an HTTP request object, stored in physical memory (PID, threads, request object address).
4. **CPU Execution** — Core 0 lights up; **live registers** (RAX…RIP) and **assembly stepping** (CALL/MOV/CMP/JMP/PUSH/POP).
5. **System Call** — `send()` opens a portal from **Ring 3 → Ring 0**; privilege switches.
6. **Kernel Networking** — execution beams flow CPU → kernel code → RAM through the call stack (`send → sock_sendmsg → tcp_sendmsg → ip_queue_xmit → dev_queue_xmit`).
7. **Memory Objects** — each allocation appears physically in RAM: socket, SKB buffer, TCP/IP headers, Ethernet frame.
8. **TCP Segment** — sequence, ack, checksum, flags, window stamped onto the packet.
9. **IP Routing** — the routing engine looks up the destination and adds the IP header.
10. **NIC Driver** — `ndo_start_xmit()` programs the descriptor ring across the PCIe bus.
11. **NIC Firmware** — deep zoom into the card's own processor; the DMA engine moves the frame **RAM → PCIe → PHY → port**.
12. **Interrupt** — TX-complete fires an IRQ back across the traces to the CPU; the ISR runs.
13. **TCP Handshake & The Journey** — the board fades away and the camera *leaves the machine* to follow the packets. Frames ride **animated electromagnetic waves** out across the Internet (**PC → router → ISP → undersea fiber → Google edge → server**) through the full TCP lifecycle: **SYN → SYN-ACK → ACK** three-way handshake, then the HTTP request is **segmented** into numbered TCP packets, streamed, and **reassembled in sequence order** at the server, with the **200 OK** reply traveling home. A live handshake ladder, TCP-state HUD, and reassembly buffer track it.

---

## Interaction

Everything on screen is clickable — playback pauses so you can read the detail.

- **Timeline scrubber** — drag to any moment.
- **Play / Pause**, **Restart**, **Prev / Next scene**.
- **Slow-motion** — 0.25× / 0.5× / 1× / 2×.
- **Click any board component** — CPU, RAM, NIC, chipset, VRM, cooler, SSD, PCIe slot, RJ45, BIOS chip, CMOS battery, ATX/SATA/USB connectors, capacitors, even the SMD passives — to open a detailed **inspector** with realistic specs and its role.
- **Click any packet or network node** in the journey — each SYN/SYN-ACK/ACK, data segment, and reply opens its full header breakdown (TCP ports/seq/flags/window, IP src/dst/TTL, Ethernet MACs, payload); each hop explains its job (NAT, BGP, anycast load balancing…).
- **Editable URL** — change the `GET` field at the bottom and the host flows through every header and scene.

---

## Visual system

- **Aesthetic:** dark sci-fi HUD — deep navy substrate, holographic glass panels, neon execution beams.
- **Layer color coding:** HTTP `cyan` · TCP `green` · IP `amber` · Ethernet `violet` · IRQ `red`.
- **Type:** Space Grotesk (display) + IBM Plex Mono (technical readouts).
- Depth is rendered with cinematic CSS-3D, layered shading, and glow — every element stays clickable and every readout stays crisp at any zoom.

---

## Tech notes

- **Format:** Design Component (`<x-dc>` template + a `Component extends DCLogic` class), assembled into a standalone HTML document. Loads its fonts from Google Fonts; otherwise fully offline-capable.
- **Engine:** a single master timeline drives a `requestAnimationFrame` clock. Each frame computes a camera (smooth eased fly between component focus points), projects board-space coordinates to screen space for the holographic UI, and renders the active scene's panels, beams, and board-space elements.
- **No external libraries**, no bundler, no server. Just open the file.

---

## Running locally

```bash
# clone, then simply open the file in a browser:
open "Packet Journey v2.dc.html"      # macOS
# or double-click it in your file explorer
```

For the smoothest experience use a Chromium-based browser at a desktop window size.

---

*Built with Claude.*
