# 03 · Replay Attack — Capture & Re-transmit Control Packets

> ⚠️ See the [DISCLAIMER](../DISCLAIMER.md). Performed only on the author's own drone, in a controlled environment, at minimal power. **Do not transmit on signals you do not own or are not authorized to test.**

## Concept

A **replay attack** records legitimate transmissions and re-sends them later to make a system repeat an action — **without ever needing to understand or decrypt the payload.** If a control link lacks **replay protection** (rolling codes, nonces, timestamps, or sequence counters), a recorded "throttle up" packet will be accepted again whenever it is replayed.

```
   Controller ──(throttle up)──►  Drone        [1] legitimate command, recorded by SDR
                                     ▲
   SDR (HackRF) ──(replayed pkt)─────┘          [2] same bytes re-transmitted later
                                                    → drone reacts again = VULNERABLE
```

## Procedure

### 1. Capture (passive)
- Tune the HackRF to the control frequency found during recon (~2440 MHz region).
- Using Mayhem's **Capture** app, record the raw baseband while performing **one isolated command** (e.g. throttle up, yaw, directional move).
- Label each capture by the action it represents — one file per command keeps the mapping clean.

### 2. Replay (active)
- Open the captured file in Mayhem's **Replay** app.
- Ensure the genuine controller is **not** issuing that command.
- Transmit the capture on the same frequency at low power.

### 3. Observe
- The drone **responded to the replayed transmission and reproduced the captured movement** — with no corresponding input from the real controller.

## Result

✅ **Replay attack successful.** The drone executed movement driven purely by re-transmitted packets.

### What this proves
- **No replay protection** on the control link — identical packets are accepted repeatedly.
- **No (effective) encryption or authentication** preventing a third-party transmitter from being trusted.
- An attacker within range could **inject control actions** or, with enough captured commands, meaningfully **influence flight**.

This breaks the **integrity** of the control channel.

## Limitations & honesty

- Replay reproduces **previously captured actions**; it is not the same as full, real-time joystick-level hijacking of an arbitrary flight path.
- Half-duplex SDR timing, antenna, range, and gain all affect reliability.
- Results are specific to the tested consumer-grade drone; better-designed links (FHSS + crypto) resist this.

## Defence

Replay attacks are defeated by **rolling codes / nonces / sequence numbers + authenticated encryption**. See [05-findings-and-mitigations.md](05-findings-and-mitigations.md).
