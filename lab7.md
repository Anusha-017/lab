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

### 3.4. Conclusion
**Final Output Expression:**
$$V_{out}(t) \approx 0.637 \cos(5000 \pi t) \text{ V}$$

The theoretical analysis predicts an output waveform with a peak amplitude of approximately **637 mV**, shifted by **+90°** (cosine wave) relative to the input sine wave.
## 4. DC Analysis

<img width="1577" height="657" alt="image" src="https://github.com/user-attachments/assets/83fdef73-010e-414c-ba97-0559cae48a88" />


The DC analysis of a practical integrator is essential to ensure the Op-Amp remains in the linear region and does not saturate due to input offset voltages or bias currents.

### 4.1 Role of the Feedback Resistor ($R_1$)
In an ideal integrator, the feedback path consists only of a capacitor ($C_1$). At DC ($f = 0$), a capacitor acts as an open circuit, making the DC gain infinite ($A_v = -Z_f / R_{in}$). This would cause the Op-Amp to saturate at the supply rails ($V_{CC}$ or $V_{EE}$) even with a micro-volt DC offset.

By adding $R_1 = 10k\Omega$ in parallel with $C_1$, we limit the maximum DC gain to:
$$A_{V(DC)} = -\frac{R_1}{R_2} = -\frac{10k\Omega}{1k\Omega} = -10$$

### 4.2 Operating Point Results
From the LTspice `.op` simulation, the following DC parameters were observed:

| Parameter | Simulated Value | Description |
| :--- | :--- | :--- |
| **V(n002)** | $+12.0V$ | Positive Supply Rail ($V_{CC}$) |
| **V(n004)** | $-12.0V$ | Negative Supply Rail ($V_{EE}$) |
| **V(vin1)** | $0V$ | DC component of the input signal |
| **V(vout)** | $\approx 211.3 \mu V$ | Steady-state DC output offset |

### 4.3 Bias Current Compensation
The resistor $R_4$ (connected to the non-inverting terminal) is chosen to minimize the output offset voltage caused by the input bias current ($I_b$). For optimal compensation:
$$R_4 = R_1 \parallel R_2 = \frac{R_1 \cdot R_2}{R_1 + R_2}$$
$$R_4 = \frac{10k \cdot 1k}{10k + 1k} \approx 909\Omega$$

The simulation uses **$0.91k\Omega$**, which effectively matches the parallel combination and results in a very low DC output voltage ($\approx 0.2mV$), confirming the circuit is well-biased and centered.

### 4.4 Input Bias and Offset Currents
Based on the `.op` data for the $\mu A741$:
* **$I(R2)$:** $-53.29 nA$ (Current flowing into the inverting terminal)
* **$I(R4)$:** $79.72 nA$ (Current flowing into the non-inverting terminal)
The difference between these currents represents the Input Offset Current ($I_{os}$) specific to the model parameters used in the `lm741.lib`.


## 5.Transient  Analysis

<img width="1917" height="581" alt="image" src="https://github.com/user-attachments/assets/d2ded376-a8fe-4d9f-89b6-6cf474f9f9c1" />

### 5.1. Startup Transient and Settling Time
When the simulation begins at $t = 0 \text{ s}$, the feedback capacitor ($C_F$) holds zero charge. The sudden application of the input signal causes an initial DC offset in the output voltage, visibly dropping to nearly $-1.15 \text{ V}$ in the first half-millisecond.

The circuit utilizes the feedback resistor ($R_F$) to bleed off this initial charge. The speed of this recovery is dictated by the time constant ($\tau$) of the feedback loop:
$$\tau = R_F \times C_F = 10 \text{ k}\Omega \times 100 \text{ nF} = 1 \text{ ms}$$

Standard circuit theory states that a transient will fully settle after approximately 5 time constants ($5\tau$). 
* **Predicted Settling Time:** $5 \times 1 \text{ ms} = 5 \text{ ms}$

**Observation:** The provided simulation waveform precisely corroborates this. The drift heavily attenuates over the first few milliseconds, and by the $5 \text{ ms}$ mark, the waveform is visibly stabilized and cleanly centered around the $0 \text{ V}$ baseline.


### 5.2. Phase Relationship (Integration)

The most important observation is the phase shift.

* **Input ($V_{in1}$):** A sine wave starting at $0\text{V}$ and going positive.
* **Output ($V_{out}$):** A waveform that peaks when the input crosses zero. Mathematically, the integral of $\sin(\omega t)$ is $-\cos(\omega t)$. In your graph, the output is a **negative cosine wave** (it starts at $0\text{V}$ and immediately drops toward a negative peak). This confirms that the circuit is successfully performing the mathematical operation of integration.


## 6. AC Analysis 

### . Simulation Parameters
* **Analysis Type:** AC Sweep (`.ac dec 100 1 100k`)
* **Sweep Type:** Decade (100 points per decade)
* **Frequency Range:** $1 \text{ Hz}$ to $100 \text{ kHz}$
* **Input Voltage:** $1 \text{ V}$ (AC Amplitude)
* **Circuit Configuration:** Practical Integrator ($R_{in}=1\text{k}\Omega, R_F=10\text{k}\Omega, C_F=100\text{nF}$)

  <img width="1833" height="845" alt="image" src="https://github.com/user-attachments/assets/0fd45530-4381-46cf-950c-10321bf2b9ed" />


###  Results and Observations

### 6.1. Low-Frequency Gain (Bandband)
At low frequencies (below $100 \text{ Hz}$), the capacitor $C_F$ acts as an open circuit. The gain is dominated by the resistors:
* **Theoretical DC Gain ($A_v$):** $20 \log_{10}(R_F / R_{in}) = 20 \log_{10}(10\text{k} / 1\text{k}) = 20 \text{ dB}$.
* **Observation:** The simulation shows a flat passband at exactly **$20 \text{ dB}$**.

### 6.2. Cutoff Frequency ($f_c$)
The cutoff frequency is the point where the gain drops by $3 \text{ dB}$ from the maximum ($20 \text{ dB} - 3 \text{ dB} = 17 \text{ dB}$).
* **Theoretical $f_c$:** $1 / (2 \pi R_F C_F) \approx 159.15 \text{ Hz}$.
* **Observation (Cursor 1):** At $155.22 \text{ Hz}$, the magnitude is **$17.09 \text{ dB}$**. This confirms the theoretical cutoff frequency with high precision.

### 6.3. Integration Region and Roll-off
For frequencies $f > f_c$, the circuit behaves as an integrator.
* **Magnitude Response:** The plot shows a constant slope of **$-20 \text{ dB/decade}$**.
* **Phase Response:** The phase starts at $180^\circ$ (inverting) and shifts toward $90^\circ$ as frequency increases. At $1.55 \text{ kHz}$ (Cursor 2), the phase is **$95.7^\circ$**, which is nearly the ideal $90^\circ$ required for mathematical integration.

### 6.4. Unity Gain Frequency ($f_{unity}$)
The frequency where the gain reaches $0 \text{ dB}$ is the unity-gain frequency.
* **Theoretical $f_{unity}$:** $1 / (2 \pi R_{in} C_F) = 1 / (2 \pi \cdot 10^3 \cdot 100 \cdot 10^{-9}) \approx 1.59 \text{ kHz}$.
* **Observation:** The plot crosses $0 \text{ dB}$ (magnitude axis) just past the $1.5 \text{ kHz}$ mark, matching the calculation.







