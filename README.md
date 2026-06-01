# Amplifier and Crossover Filter Network

---

## Overview

This project designs and implements an amplifier combined with a crossover filter network for audio signal processing. The system amplifies an input audio signal by at least 10 dB, then splits it into two frequency bands — a low-end (woofer) channel and a midrange channel — using second-order Chebyshev filters.

---

## Signal Chain

```
Audio Input → Non-Inverting Amplifier (gain 11) → LPF (fc = 305 Hz) → Buffer → Speaker (low)
                                                 → HPF (fc = 2.1 kHz) → Buffer → Speaker (mid)
```

---

## Amplifier Design

A non-inverting op-amp configuration was used to achieve a minimum gain of 10 dB (gain = 11):

```
G ≥ 10 ≥ 1 + R₂/R₁  →  R₂ ≥ 9·R₁
```

A buffer was added at the output of each filter to prevent loading error.

---

## Target Frequencies

FFT analysis of the audio signal revealed three frequency components:

| Band     | Frequency  |
|----------|-----------|
| Low-end  | ~261.6 Hz |
| Midrange | ~830.6 Hz |
| High-end | ~2489.2 Hz |

**Cutoff frequencies chosen:**
- LPF: **305 Hz** (passes low-end, blocks midrange and above)
- HPF: **2.1 kHz** (passes high-end, blocks midrange and below)

Cutoff frequencies were determined using [Analog Filter Wizard](https://www.analog.com/designtools/en/filterwizard/).

---

## Filter Type

Both filters are **second-order Chebyshev** designs, chosen for:
- Compact component count (lower order filter achieves target cutoff)
- Fast roll-off relative to Butterworth designs
- Acceptable passband ripple for audio applications

---

## Component Lists

### Low-Pass Filter (LPF)

| Component | Quantity |
|-----------|----------|
| 402 Ω     | 1        |
| 100 Ω     | 1        |
| 10 Ω      | 5        |
| 3 Ω       | 1        |
| 1 Ω       | 1        |
| 10 µF     | 1        |
| 1 µF      | 1        |
| LM741 op-amp | 1     |

### High-Pass Filter (HPF)

| Component | Quantity |
|-----------|----------|
| 1 kΩ      | 1        |
| 402 Ω     | 1        |
| 100 Ω     | 2        |
| 10 Ω      | 1        |
| 100 nF    | 2        |
| LM741 op-amp | 1     |

---

## Analog vs. Digital Filters

| | Analog | Digital |
|---|---|---|
| Latency | Low | Higher |
| Ease of use at low freq. | High | Moderate |
| Precision | Limited | High |
| Component variation immunity | Low | High |
| Environmental immunity | Low | High |

> **Nyquist note:** With f_max = 2500 Hz, the minimum sampling rate for A/D conversion is ~5000 samples/sec to prevent aliasing.

---

## Implementation

The circuit was modeled in **MATLAB Simscape** before physical breadboard implementation. Both implementations successfully split the audio signal into two frequency bands.

**Challenges encountered:**
- Simscape: simulation files placed in incorrect folders
- Physical: initial assembly failed to output audio; required debugging of the Chebyshev filter stages

---

## Demo Videos (Appendices)

- [Simscape simulation](https://youtube.com/watch/vOlNdGs_ma8)
- [Low-Pass Filter demo](https://youtu.be/wrvXswK2nsc)
- [High-Pass Filter demo](https://youtu.be/BuL4UwbUtf8)

---

## References

1. Miller, S. (2020). *Analog vs. digital filtering of data.* Tufts University EE Senior Design Handbook. https://sites.tufts.edu/eeseniordesignhandbook/files/2020/05/Miller_MaxBlueGreen_Tech-note.pdf
