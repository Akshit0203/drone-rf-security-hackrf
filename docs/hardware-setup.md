# Hardware & Software Setup

> ⚠️ See the [DISCLAIMER](../DISCLAIMER.md).

## Bill of Materials

| Item | Description | Notes |
|------|-------------|-------|
| **HackRF One** | Half-duplex SDR transceiver, 1 MHz – 6 GHz, up to 20 Msps | The RX/TX engine |
| **PortaPack H2+** | Add-on board with 2.8" touchscreen, controls, RTC & battery | Makes the HackRF standalone/portable |
| **Mayhem firmware** | Community firmware for the PortaPack | Adds Looking Glass, Capture, Replay, Jammer, etc. |
| **2.4 GHz antenna** | SMA telescopic/whip antenna | Tuned for the ISM band |
| **Target drone** | Consumer quadcopter (E99 Pro-class) | Author-owned test target |
| **Stock controller** | The drone's bundled 2.4 GHz transmitter | Source of legitimate control signals |

## Software / firmware

- **PortaPack Mayhem firmware** — flash per the official project instructions:
  https://github.com/portapack-mayhem/mayhem-firmware
- Key Mayhem apps used:
  - **Looking Glass** — wideband spectrum/waterfall analyzer (reconnaissance)
  - **Capture** — record raw baseband
  - **Replay** — re-transmit a captured file
  - **(CW) transmit / Jammer** — interference testing

> Optional desktop workflow (not required here): `hackrf_tools`, GNU Radio, GQRX, Universal Radio Hacker (URH), and `inspectrum` for offline analysis.

## Device settings used in this project

| Setting | Value |
|---------|-------|
| Sweep window | 2320 – 2560 MHz (`RANGE 240`) |
| Band preset | `BLE / WIFI 2.4GHz` |
| Marker | ~2440.3125 MHz |
| LNA gain | 32 |
| VGA gain | 32 |
| External AMP | 0 (off) |

> Gains should be tuned per environment — too high clips the signal and raises the noise floor; start moderate and adjust.

## Safety & compliance checklist

- [ ] Only your **own** equipment is involved.
- [ ] Testing is in a **controlled / low-power** setup.
- [ ] TX time is **minimal** and limited to proof-of-concept.
- [ ] No third-party devices, networks, or spectrum users are affected.
- [ ] You comply with **local RF and aviation law** (licensing, no-jamming, Remote ID, no-fly zones).

See [references.md](references.md) for tooling and background reading.
