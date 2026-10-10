# 04 · Jamming — Continuous-Wave Interference (Denial of Service)

> 🚨 **CRITICAL LEGAL WARNING** — Jamming is **illegal in most countries** (in the US it violates the *Communications Act*; the FCC bans the use, marketing, and sale of jammers — equivalent bans exist in the EU, UK, India, and elsewhere). This experiment was conducted **only on the author's own drone, in a controlled environment, at minimal power, for a few seconds.** It is documented here to explain the **threat and its defence**, not as an instruction to jam anything. See the [DISCLAIMER](../DISCLAIMER.md).

## Concept

A **jamming** attack targets **availability**. Instead of understanding the signal, the attacker simply floods the control band with **interference**, raising the noise floor so the drone can no longer cleanly receive commands. A **continuous-wave (CW)** jammer transmits a persistent carrier over the target frequency.

```
   Controller ──(weak control signal)──►  Drone
   Jammer     ══════(strong CW noise)════►  ▲
                                            │  receiver can't decode commands
                                            ▼
                               link lost → FAILSAFE (instability / forced landing)
```

## Procedure (conceptual)

- Identify the control band (2.4 GHz, from recon).
- Emit CW interference over that band using the SDR for a **brief** period.
- Observe the drone's response to command loss.

## Result

⚠️ The control link **degraded**: commands became unreliable and the drone exhibited **instability and entered a failsafe / forced-landing** state.

### What this proves
- A **trivial, low-cost DoS** can disrupt the control link of a consumer drone.
- The drone's **failsafe behaviour** is the last line of defence when the link drops — its quality (controlled landing vs. uncontrolled crash) is a critical safety property.

## Why it works

Consumer drones on a single/narrow 2.4 GHz channel present a small, predictable target. Without **spread-spectrum / frequency hopping**, all the energy an attacker needs is concentrated in one place.

## Defence

- **FHSS / DSSS** spread-spectrum links make the signal far harder to jam (an attacker must jam a much wider band).
- **Robust failsafe logic** — predictable return-to-home / controlled descent on link loss.
- **Link-quality monitoring** and multi-band / redundant links for critical UAVs.

See [05-findings-and-mitigations.md](05-findings-and-mitigations.md).

## Safety reminder

Never test jamming on shared spectrum, near other people's devices, or anywhere that could affect Wi-Fi, Bluetooth, medical, or aviation systems. Interference can have serious safety consequences and legal penalties.
