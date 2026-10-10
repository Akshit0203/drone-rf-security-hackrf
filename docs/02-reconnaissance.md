# 02 · Reconnaissance — Spectrum Analysis with Looking Glass

> ⚠️ See the [DISCLAIMER](../DISCLAIMER.md). Reconnaissance here is **passive** — receive only.

## Goal

Before any capture or transmission, we need to **find** the drone's control link in the spectrum and confirm which bursts correspond to operator input.

## Tool: Looking Glass

**Looking Glass** is a wideband spectrum/waterfall analyzer in the PortaPack **Mayhem** firmware. It rapidly sweeps a frequency window and renders a **waterfall** — time scrolling vertically, frequency across the horizontal axis, and signal strength encoded by colour (blue = noise floor → green/yellow/red = stronger energy).

## Configuration used

| Parameter | Value | Meaning |
|-----------|-------|---------|
| `MIN` | 2320 MHz | Lower edge of sweep |
| `MAX` | 2560 MHz | Upper edge of sweep |
| `RANGE` | 240 | Width of the swept window (MHz) |
| `LNA` | 32 | Low-noise amplifier gain |
| `VGA` | 32 | Variable-gain amplifier gain |
| `AMP` | 0 | External RF amp off |
| `MARKER` | ~2440.3125 MHz | Frequency of interest |
| Preset | `BLE / WIFI 2.4GHz` | 2.4 GHz ISM band preset |

This window covers the **2.4 GHz ISM band**, shared by Wi-Fi, Bluetooth/BLE, and the proprietary control links used by many drones.

## Reading the waterfall

1. With the controller **off / idle**, the band shows the ambient noise floor plus background Wi-Fi/BLE activity.
2. When the controller **sticks or buttons are actuated**, **distinct bright vertical bursts** appear — energy keying on in sync with input.
3. Because the bursts **start and stop with operator action**, they can be attributed to the **control uplink** rather than unrelated ambient traffic.

See the captured evidence:

- `../media/images/03-signal-bursts-highlighted.jpg` — Looking Glass waterfall of the swept window with the control bursts **highlighted in red boxes**.

## Why this matters

The fact that the control channel is **visible, distinguishable, and locatable** with a low-cost SDR is the first security takeaway: the link offers **no transmission-level obscurity** and is a practical target for capture. This directly motivates **frequency-hopping (FHSS)** and **encryption** as defences (see [05-findings-and-mitigations.md](05-findings-and-mitigations.md)).

## Next step

With the center frequency identified, proceed to passive capture and then the replay test → [03-replay-attack.md](03-replay-attack.md).
