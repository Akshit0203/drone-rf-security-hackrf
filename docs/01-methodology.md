# 01 · Methodology — SDR RF Attack Kill-Chain

> ⚠️ Read the [DISCLAIMER](../DISCLAIMER.md) first. All steps below were performed on the author's own equipment, in a controlled environment, at minimal power.

This document gives the end-to-end workflow used to assess the drone's 2.4 GHz control link. Each phase maps to a classic security kill-chain stage adapted to the RF domain.

```
┌───────────────┐   ┌──────────────┐   ┌───────────────┐   ┌──────────────┐
│ 1. RECON      │ → │ 2. INTERCEPT │ → │ 3. REPLAY     │ → │ 4. JAM / DoS │
│ Spectrum scan │   │ Passive capt │   │ Re-transmit   │   │ CW interfere │
│ Looking Glass │   │ raw bursts   │   │ control pkts  │   │ same band    │
└───────────────┘   └──────────────┘   └───────────────┘   └──────────────┘
      │                    │                   │                   │
  Confidentiality     Confidentiality      Integrity           Availability
```

## Why SDR?

A Software-Defined Radio moves signal processing from fixed hardware into software, so a single device can **receive, record, and transmit** across a wide frequency range. The **HackRF One** (1 MHz – 6 GHz, half-duplex) paired with the **PortaPack H2+** and **Mayhem firmware** turns this into a **portable, standalone RF workbench** — no laptop required. This makes it an ideal platform to study the real-world wireless attack surface of consumer devices.

## Phase 1 — Reconnaissance

**Objective:** locate the drone's control link in the spectrum.

- Launch **Looking Glass** (Mayhem's wideband spectrum/waterfall analyzer).
- Sweep the 2.4 GHz ISM band — window set to **2320–2560 MHz** (`RANGE 240`).
- Actuate the controller (sticks/buttons) and watch for **bursts that correlate with input** — the control uplink keying on and off.
- Note the center frequency of interest (observed marker **~2440.3125 MHz**).

➡️ Details: [02-reconnaissance.md](02-reconnaissance.md)

## Phase 2 — Interception (passive capture)

**Objective:** record the control transmissions without interacting with the link.

- Tune the SDR to the identified center frequency.
- Set gains for a clean capture (`LNA 32 · VGA 32 · AMP 0` as a starting point, adjusted to avoid clipping).
- Use Mayhem's **Capture** app to record the raw baseband while performing **one discrete command at a time** (e.g. throttle up, yaw left), so each capture maps to a known action.
- Interception alone already breaks **confidentiality** — the control channel is observable.

## Phase 3 — Replay Attack

**Objective:** test whether recorded packets can drive the drone.

- Load a previously captured command with Mayhem's **Replay** app.
- Re-transmit it on the same frequency with the HackRF while the genuine controller is **not** issuing that command.
- Observe whether the drone reacts. In this project the drone **reproduced the captured movement**, confirming **no replay protection** (no rolling code / nonce / sequence validation).
- This breaks **integrity** — an attacker can inject control actions.

➡️ Details: [03-replay-attack.md](03-replay-attack.md)

## Phase 4 — Jamming (Denial of Service)

**Objective:** test link availability under interference.

- Emit **continuous-wave (CW)** interference over the control band.
- Observe the effect on command responsiveness.
- Result: the link degraded and the drone entered **instability / failsafe-landing** behaviour.
- This breaks **availability** — a trivial DoS against the control link.

➡️ Details: [04-jamming.md](04-jamming.md)

## From findings to defence

Each broken property maps directly to a defensive control (encryption, replay protection, mutual auth, FHSS, robust failsafe). See **[05-findings-and-mitigations.md](05-findings-and-mitigations.md)**.
