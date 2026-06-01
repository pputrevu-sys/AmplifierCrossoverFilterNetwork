# Amplifier + Crossover Filter Network

---

## Overview

This project covers the design and implementation of an amplifier combined with a crossover filter network, developed as part of an audio engineering lab. The system targets specific audio frequencies and balances component efficiency, signal quality, and cost.

---

## Design Choices

Key considerations that drove the design decisions:

- **Amplification Requirement** — Phase distortion was a primary concern; hitting 0 dB at target frequencies was essential.
- **Component Minimization** — Filter circuits were kept compact to prevent overdraw of resources.
- **Speed of Attenuation** — Longer signal processing can cause overheating in filter components; this was mitigated by design.
- **Buffer** — Implemented to prevent loading error between stages.

---

## Design Specifications

### Target Frequencies (Audio 10)

| Note | Frequency |
|------|-----------|
| C4   | 261.595 Hz |
| G#4  | 830.585 Hz |
| D#5  | 2489.15 Hz |

### Cutoff Frequencies

- **Low-Pass Filter (LPF):** fc = 305 Hz
- **High-Pass Filter (HPF):** fc = 2.1 kHz

**Goals:** Minimized costs and maximized efficiency.

---

## Implementation

The circuit was modeled and simulated using **Simscape** (MATLAB/Simulink). The simulation covered:

- Full crossover filter network layout
- Low-Pass Filter response
- High-Pass Filter response

---

## Analog vs. Digital Filters

| | Analog Filters | Digital Filters |
|---|---|---|
| **Latency** | Low | Higher |
| **Implementation** | Easier at low frequencies | More complex |
| **Precision** | Limited | High |
| **Environmental Immunity** | Lower | High |

> Minimum signal for anti-aliasing: ≈ 5,000 samples/second

