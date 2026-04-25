# Lab - 07: Op-Amp Integrator Circuit
# Practical Inverting Integrator: Design and Analysis Report

## 1. Introduction
This repository contains the design calculations and theoretical analysis for a practical inverting operational amplifier (op-amp) integrator circuit. An integrator circuit produces an output voltage that is proportional to the integral of the input voltage with respect to time.

The circuit analyzed here includes a feedback resistor ($R_F$) in parallel with the feedback capacitor ($C_F$). This modification prevents the op-amp from drifting into saturation due to DC offset currents, effectively turning the circuit into a first-order active low-pass filter with a specific cutoff frequency.

## 2. Circuit Parameters
Based on the provided schematic, the following component values and input signal characteristics are used:

| Parameter | Symbol | Value | Unit |
| :--- | :--- | :--- | :--- |
| Input Resistor | $R_1$ | 1 | $k\Omega$ |
| Feedback Capacitor| $C_F$ | 100 | $nF$ |
| Feedback Resistor | $R_F$ | 10 | $k\Omega$ |
| Compensation Resistor | $R_{comp}$ | 0.91 | $k\Omega$ |
| Input Peak Voltage| $V_p$ | 1 | $V$ |
| Input Frequency | $f$ | 2.5 | $kHz$ |

*Note: The compensation resistor ( R_{comp} ) at the non-inverting terminal is typically set to R_1 || R_F to minimize input bias current errors, but it does not affect the ideal transfer function.*

## 3. Design Calculations

### 3.1. Cutoff Frequency ($f_c$)
The cutoff frequency determines the boundary between the amplifier region and the integrator region. It is calculated using the time constant of the feedback network ($R_F$ and $C_F$):

$$f_c = \frac{1}{2 \pi R_F C_F}$$

Substituting the values:
$$f_c = \frac{1}{2 \pi \times (10 \times 10^3) \times (100 \times 10^{-9})}$$
$$f_c \approx 159.15 \text{ Hz}$$

### 3.2. Operating Region Analysis
For the circuit to accurately act as an integrator, the input frequency ($f$) must be significantly higher than the cutoff frequency ($f_c$).

* **Input Frequency:** $f = 2500 \text{ Hz}$
* **Cutoff Frequency:** $f_c \approx 159.15 \text{ Hz}$

Since $f \gg f_c$ (2500 Hz is more than a decade above 159 Hz), the circuit operates deeply within the integration region. Therefore, we can accurately use the ideal integrator approximation.

### 3.3. Output Voltage Calculation
The input signal is a sine wave:
$$V_{in}(t) = V_p \sin(2 \pi f t)$$

The ideal integrator output equation is:
$$V_{out}(t) = -\frac{1}{R_1 C_F} \int V_{in}(t) dt$$

Substituting $V_{in}(t)$:
$$V_{out}(t) = -\frac{1}{R_1 C_F} \int V_p \sin(\omega t) dt$$
$$V_{out}(t) = \frac{V_p}{\omega R_1 C_F} \cos(\omega t)$$

Where the angular frequency $\omega$ is:
$$\omega = 2 \pi f = 2 \pi \times 2500 = 5000 \pi \approx 15708 \text{ rad/s}$$

Calculating the peak output voltage ($V_{out\_p}$):
$$V_{out\_p} = \frac{1}{(5000 \pi) \times (1 \times 10^3) \times (100 \times 10^{-9})}$$
$$V_{out\_p} = \frac{1}{5000 \pi \times 10^{-4}} = \frac{1}{0.5 \pi} = \frac{2}{\pi} \approx 0.6366 \text{ V}$$

### 4. Conclusion
**Final Output Expression:**
$$V_{out}(t) \approx 0.637 \cos(5000 \pi t) \text{ V}$$

The theoretical analysis predicts an output waveform with a peak amplitude of approximately **637 mV**, shifted by **+90°** (cosine wave) relative to the input sine wave.


