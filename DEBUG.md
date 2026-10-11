# Debug — LazyDragon Oculus-16 Analog IR Sensor Array

What to expect from the Oculus-16 hardware and how to isolate common problems.
For the hardware overview see [`README.md`](README.md); for the interface
details see [`MANUAL.md`](MANUAL.md).

> **Status:** the reader ships as `code/oculus_16.ino`. Symptoms below assume
> that sketch; firmware-specific notes are called out where relevant.

---

## Expected behavior

**Boot / startup:** with +5 V applied, the two onboard yellow power LEDs light
and the emitters draw current. The board has no onboard controller of its own —
run `code/oculus_16.ino` on a host MCU (e.g. an Arduino Nano), which drives
`S0`..`S3` and reads `C0`.

**Normal operation:** for each 4-bit address, the mux presents exactly one
sensor on `C0`. Sliding the array over a black line makes the channels directly
above the line read high (once calibrated) and the rest read low.

**Edge cases:**
- **No select change / stuck address:** `C0` mirrors one fixed sensor
  regardless of `S0`..`S3`.
- **Line off both ends:** the extreme channels (`Y0`, `Y15`) sit at their
  floor value.

---

## Quick checks

- Confirm **+5 V** and **GND** are present on `H2` (pins 7 and 1).
- Confirm `S0`..`S3` actually toggle between 0 V and 5 V with a multimeter or
  scope while the host scans.
- Confirm `C0` changes level as the array moves over a reflective surface.
- Note the mux `E#` enable is tied to GND on the board (always enabled) — no
  host wiring is needed.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| All channels read the same value regardless of `S0`..`S3` | Select lines not wired / not toggling | Verify `S0`..`S3` at `H2` pins 2–5 (`S3,S2,S1,S0`) |
| `C0` reads near 0 everywhere | Emitters not powered, sensors blocked, or sensor too far from surface | Check +5 V and emitter resistors; move array closer |
| `C0` reads near 5 V everywhere | Sensor too close / saturated, or surface highly reflective | Increase gap; check for saturation |
| Channels bleed into each other | ADC sampling before the mux path settles | Add a settle delay after selecting a channel (see **MANUAL**) |
| Reading drifts with motor load | Weak/noisy +5 V or ground drop on `C0` return | Use a clean regulated supply; star-ground the ADC |
| One channel always dead | Loose/damaged QRE1113GR or cold solder joint | Reflow or replace that sensor |

---

## Known limitations / errata

- **Channels are sequential, not simultaneous.** Only one channel is on `C0` at
  a time; scan latency must be accounted for in fast control loops.
- **Mux on-resistance.** The selected path adds ~80 Ω (typ. @ 4.5 V) in series
  with each channel; budget ADC acquisition time accordingly.


---

## Support

Open an issue in the project repository, or contact
**contact@lazydragon.club**.
