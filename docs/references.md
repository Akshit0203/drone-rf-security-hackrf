# References & Further Reading

> ⚠️ See the [DISCLAIMER](../DISCLAIMER.md). Links are provided for educational background.

## Hardware & firmware

- **HackRF One** — Great Scott Gadgets: https://greatscottgadgets.com/hackrf/
- **PortaPack Mayhem firmware** (GitHub): https://github.com/portapack-mayhem/mayhem-firmware
- **Mayhem wiki / docs**: https://github.com/portapack-mayhem/mayhem-firmware/wiki

## SDR tooling (desktop, optional)

- **GNU Radio** — SDR signal-processing framework: https://www.gnuradio.org/
- **GQRX** — SDR receiver: https://gqrx.dk/
- **Universal Radio Hacker (URH)** — protocol analysis & attacks on wireless: https://github.com/jopohl/urh
- **inspectrum** — offline capture analysis: https://github.com/miek/inspectrum
- **hackrf tools** — `hackrf_transfer`, `hackrf_sweep`, etc.

## Background — RF / drone security

- **The Great Scott Gadgets "SDR with HackRF" course** — free lessons on SDR fundamentals.
- **FHSS / DSSS spread spectrum** — foundational anti-jam / anti-intercept techniques.
- **Replay attacks & rolling codes** — classic study material on why sequence/rolling codes matter.
- **Remote ID (FAA / EASA / DGCA)** — regulatory background on drone identification and airspace safety.

## Regulatory / legal (know before you transmit)

- **FCC — Jammer Enforcement** (US): the sale, use, and marketing of signal jammers is prohibited.
- Equivalent national regulators: **Ofcom** (UK), **ANFR/ETSI** (EU), **WPC/DoT & DGCA** (India).
- Aviation authorities govern UAV operation (**FAA Part 107**, **EASA**, **DGCA**, etc.).

> ⚖️ Laws vary by country and change over time. This list is informational, not legal advice. Always verify the current rules for your jurisdiction.

## Related concepts

- Software-Defined Radio (SDR)
- ISM band (2.4 GHz / 5 GHz)
- Waterfall / spectrogram analysis
- Replay attack, spoofing, jamming (DoS)
- AEAD encryption (AES-GCM), mutual authentication, nonces/rolling codes
