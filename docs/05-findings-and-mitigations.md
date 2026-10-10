# 05 · Findings & Mitigations

> ⚠️ See the [DISCLAIMER](../DISCLAIMER.md).

## Summary of findings

| # | Finding | Security property broken | Severity |
|---|---------|--------------------------|----------|
| F1 | Control-link bursts are **visible and locatable** with a hobby SDR | Confidentiality | Medium |
| F2 | Control transmissions can be **passively captured** | Confidentiality | Medium |
| F3 | Captured packets can be **replayed to reproduce drone movement** | Integrity | **High** |
| F4 | The link lacks **encryption / mutual authentication** (observed) | Confidentiality + Integrity | **High** |
| F5 | **CW jamming** disrupts the link → instability / failsafe | Availability | **High** |

### Root cause

The tested consumer drone uses an **unencrypted, unauthenticated, non-replay-protected** control link on a **single/narrow 2.4 GHz channel**. This combination makes interception, replay, and jamming individually straightforward.

## Threat model

- **Attacker position:** within RF range (meters to tens of meters with a basic antenna).
- **Attacker cost:** low — a single ~hobby-grade SDR.
- **Attacker skill:** moderate — standard SDR tooling, no custom silicon.
- **Impact:** loss of control-channel confidentiality, injected/replayed commands, and denial of service that can force an unsafe landing.

## Mitigations (for manufacturers / designers)

### 1. Authenticated encryption on the control link 🔐
Encrypt control payloads with a modern AEAD cipher (e.g. AES-GCM). Captured packets become opaque and cannot be forged → defeats **F2, F4**.

### 2. Replay protection 🔁
Add **rolling codes, nonces, timestamps, or monotonic sequence counters**, validated by the drone. A replayed packet is detected as stale and rejected → defeats **F3**.

### 3. Mutual authentication 🤝
Cryptographically **bind controller ↔ drone** so only an authenticated transmitter is trusted. Stops rogue transmitters → defeats **F4**.

### 4. Frequency-Hopping Spread Spectrum (FHSS) 📡
Hop across many channels on a shared secret schedule. This dramatically raises the cost of both **interception (F1/F2)** and **jamming (F5)**.

### 5. Robust failsafe & link monitoring 🛰
On link loss, execute a **safe, predictable** response (controlled hover → RTH → controlled descent) instead of an uncontrolled crash → reduces the impact of **F5**.

### 6. Accountability controls 🧭
**Remote ID**, geofencing, and firmware signing for airspace safety and tamper resistance.

## Mitigations (for operators)

- Buy drones that advertise **encrypted / FHSS control links** for sensitive use.
- Keep **firmware updated**.
- Avoid flying critical missions in **RF-congested or hostile** environments.
- Understand and test your drone's **failsafe behaviour** before relying on it.

## Defender takeaways (blue team)

- RF is an **in-scope attack surface** for any wireless/embedded product threat model.
- Spectrum monitoring can **detect** anomalous bursts or jamming.
- The security properties to verify on any control link: **confidentiality, integrity (incl. replay), authentication, and availability.**
