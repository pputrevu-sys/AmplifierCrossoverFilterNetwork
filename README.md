# Amplifier and Crossover Filter Network

 **Author:** Prerana Putrevu

---

## Overview

This project designs and implements an amplifier combined with a crossover filter network for audio signal processing. The system amplifies an input audio signal by at least 10 dB, then splits it into two frequency bands — a low-end (woofer) channel and a midrange channel — using second-order Chebyshev filters modeled in MATLAB Simscape.

---

## Repository Structure

```
AmplifierCrossoverFilterNetwork/
├── *.slx               # Simscape circuit models (see below)
├── .gitattributes      # Marks .slx files as binary for Git
└── README.md
```

---

## Installing MATLAB and Simulink

### Step 1 — Create a MathWorks account

Go to [mathworks.com](https://www.mathworks.com) and create a free account, or sign in with your university credentials if your institution provides a campus license (Stevens students: check [my.stevens.edu](https://my.stevens.edu) for access).

### Step 2 — Download the installer

1. Sign in at [mathworks.com/downloads](https://www.mathworks.com/downloads)
2. Select the latest MATLAB release (R2024b or newer recommended)
3. Download the installer for your OS (Windows, macOS, or Linux)

### Step 3 — Run the installer

Launch the installer and sign in with your MathWorks account when prompted. On the product selection screen, check **both** of these products:

- ✅ **MATLAB**
- ✅ **Simulink**

For this project, also select:

- ✅ **Simscape** — required to open and run the `.slx` circuit models
- ✅ **Simscape Electrical** — provides the electrical component library (resistors, capacitors, op-amps)
- ✅ **Signal Processing Toolbox** — used for FFT analysis

Click **Next** and complete the installation (this typically takes 10–30 minutes depending on your connection).

### Step 4 — Activate your license

When MATLAB launches for the first time, it will prompt you to activate. Choose **Activate automatically using the Internet** and sign in with your MathWorks account.

---

## Cloning the Repository

```bash
git clone https://github.com/pputrevu-sys/AmplifierCrossoverFilterNetwork.git
cd AmplifierCrossoverFilterNetwork
```

> **Note:** `.slx` files are tracked as binary in this repo via `.gitattributes`. Do not attempt to merge them manually — always use MATLAB to open and modify them.

---

## Running the Simscape Models

### Opening an `.slx` file

1. Launch **MATLAB**
2. In the MATLAB toolbar, click **Open** (or press `Ctrl+O` / `Cmd+O`)
3. Navigate to the cloned repo folder and open the `.slx` file you want to run
4. The model will open in **Simulink**

Alternatively, from the MATLAB Command Window:

```matlab
open('your_model_name.slx')
```

### Setting the working directory

Make sure MATLAB's current folder is set to the repo root so all files resolve correctly:

```matlab
cd('path/to/AmplifierCrossoverFilterNetwork')
```

Or use the **Current Folder** panel in the MATLAB GUI to navigate there.

### Running a simulation

1. With the model open in Simulink, check that the **Simulation stop time** in the toolbar is set to an appropriate value (e.g. `0.1` seconds for audio signals)
2. Click the **Run** button (▶) in the Simulink toolbar, or press `Ctrl+T`
3. Open a **Scope** block to view the output waveform — double-click any Scope in the model

### Viewing FFT results

To reproduce the FFT analysis from the paper:

1. Run the simulation
2. In the Scope window, click **Tools → Signal Processing** (or right-click the Scope → **FFT Analysis**)
3. The frequency peaks at ~261 Hz, ~830 Hz, and ~2489 Hz should be visible

---

## Signal Chain

```
Audio Input → Non-Inverting Amplifier (gain 11)
                    ├── LPF (fc = 305 Hz)  → Buffer → Speaker (low-end)
                    └── HPF (fc = 2.1 kHz) → Buffer → Speaker (midrange)
```

---

## Amplifier Design

A non-inverting op-amp configuration achieves a minimum gain of 10 dB (gain = 11):

```
G ≥ 10 ≥ 1 + R₂/R₁  →  R₂ ≥ 9·R₁
```

A buffer was added at the output of each filter to prevent loading error.

---

## Target Frequencies

FFT analysis of the audio signal revealed three frequency components:

| Band     | Frequency   |
|----------|-------------|
| Low-end  | ~261.6 Hz   |
| Midrange | ~830.6 Hz   |
| High-end | ~2489.2 Hz  |

**Cutoff frequencies chosen:**
- LPF: **305 Hz** (passes low-end, blocks midrange and above)
- HPF: **2.1 kHz** (passes high-end, blocks midrange and below)

Cutoff frequencies were determined using [Analog Filter Wizard](https://www.analog.com/designtools/en/filterwizard/).

---

## Filter Type

Both filters are **second-order Chebyshev** designs, chosen for:
- Compact component count (lower order achieves target cutoff)
- Fast roll-off relative to Butterworth designs
- Acceptable passband ripple for audio applications

---

## Component Lists

### Low-Pass Filter (LPF) — fc = 305 Hz

| Component    | Quantity |
|--------------|----------|
| 402 Ω        | 1        |
| 100 Ω        | 1        |
| 10 Ω         | 5        |
| 3 Ω          | 1        |
| 1 Ω          | 1        |
| 10 µF        | 1        |
| 1 µF         | 1        |
| LM741 op-amp | 1        |

### High-Pass Filter (HPF) — fc = 2.1 kHz

| Component    | Quantity |
|--------------|----------|
| 1 kΩ         | 1        |
| 402 Ω        | 1        |
| 100 Ω        | 2        |
| 10 Ω         | 1        |
| 100 nF       | 2        |
| LM741 op-amp | 1        |

---

## Analog vs. Digital Filters

|                              | Analog | Digital |
|------------------------------|--------|---------|
| Latency                      | Low    | Higher  |
| Ease of use at low freq.     | High   | Moderate|
| Precision                    | Limited| High    |
| Component variation immunity | Low    | High    |
| Environmental immunity       | Low    | High    |

> **Nyquist note:** With f_max = 2500 Hz, the minimum sampling rate for A/D conversion is ~5000 samples/sec to prevent aliasing.

---

## Troubleshooting

**"Library not found" error when opening `.slx`**  
You are missing a required Simscape toolbox. Go to **Home → Add-Ons → Get Add-Ons** and search for "Simscape Electrical", then install it.

**Model opens but won't simulate**  
Check that all blocks are connected (no red error highlights). Also ensure your MATLAB working directory is set to the repo folder.

**Scope shows a flat line**  
Confirm the simulation stop time is long enough (at least 10× the period of your lowest frequency: 1/261 Hz ≈ 0.004 s, so use at least `0.05` s).

**`.slx` file shows as modified in Git after just opening it**  
This is normal — MATLAB updates internal metadata on open. Revert with `git checkout -- *.slx` if you made no intentional changes.

---

## Demo Videos

- [Simscape simulation](https://youtube.com/watch/vOlNdGs_ma8)
- [Low-Pass Filter demo](https://youtu.be/wrvXswK2nsc)
- [High-Pass Filter demo](https://youtu.be/BuL4UwbUtf8)

---

## References

1. Miller, S. (2020). *Analog vs. digital filtering of data.* Tufts University EE Senior Design Handbook.  
   https://sites.tufts.edu/eeseniordesignhandbook/files/2020/05/Miller_MaxBlueGreen_Tech-note.pdf
