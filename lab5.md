# Part A : Op-Amp Circuit Design: Non-Inverting Amplifier

## 1. Theory
A **Non-Inverting Amplifier** is an operational amplifier circuit configuration that produces an output signal in phase with the input signal. The input signal is applied directly to the non-inverting (+) terminal, while a portion of the output is fed back to the inverting (-) terminal through a voltage divider network consisting of resistors $R_f$ and $R_1$.

**Key Characteristics:**
* **High Input Impedance:** Since the input is connected directly to the non-inverting terminal, the circuit draws negligible current from the source.
* **Voltage Gain:** The gain is always greater than or equal to 1.
* **Phase:** There is a $0^\circ$ phase shift between the input and output.

The closed-loop voltage gain ($A_v$) is governed by the expression:
$$A_v = 1 + \frac{R_f}{R_1}$$

---

## 2. Design Calculations

### Given Specifications:
* Supply Voltage: $V_{CC} = \pm 15\text{ V}$
* Input Voltage: $V_{in(pp)} = 12\text{ V}$ ($V_{in(peak)} = 6\text{ V}$)
* Input Frequency: $f = 2\text{ kHz}$
* Target Voltage Gain: $A_v = +5\text{ V/V}$
* Load Resistance: $R_L = 1\text{ k}\Omega$

### Step 1: Resistor Selection
Using the gain formula:
$$5 = 1 + \frac{R_f}{R_1}$$
$$4 = \frac{R_f}{R_1} \implies R_f = 4 R_1$$

To ensure the op-amp operates within its linear region and to minimize the effects of input bias currents, we typically choose resistor values in the kilo-ohm range.
* Let $R_1 = 1\text{ k}\Omega$
* Then $R_f = 4 \times 1\text{ k}\Omega = 4\text{ k}\Omega$


### Step 2: Output Voltage Calculation
Calculate the expected peak-to-peak output to ensure it does not exceed the saturation limits ($V_{sat} \approx \pm 14\text{ V}$ for a $15\text{ V}$ supply).
$$V_{out(pp)} = A_v \times V_{in(pp)}$$
$$V_{out(pp)} = 5 \times 12\text{ V} = 60\text{ V}$$

> **Critical Observation:** The calculated $V_{out(pp)}$ is $60\text{ V}$. However, the supply is only $\pm 15\text{ V}$ (total $30\text{ V}$ swing). The output will **clip** heavily at approximately $+14\text{ V}$ and $-14\text{ V}$. To observe a clean undistorted waveform with a gain of 5, the input $V_{in(pp)}$ should be reduced to below $5.6\text{ V}$.

### Step 3: Summary Table

| Parameter | Value |
| :--- | :--- |
| **Gain ($A_v$)** | $5\text{ V/V}$ |
| **Input Resistor ($R_1$)** | $1\text{ k}\Omega$ |
| **Feedback Resistor ($R_f$)** | $4\text{ k}\Omega$ |


### Circuit Diagram

<img width="607" height="395" alt="image" src="https://github.com/user-attachments/assets/3ba9a24a-274c-41cb-8ca0-d7da4947b6e5" />

---

## 3. Expected Waveforms
The output waveform will be a "flat-topped" sine wave in phase with the input. The peaks will be truncated at the op-amp's saturation rails because the requested gain multiplied by the input voltage exceeds the power supply limits.


## 4. Transient Analysis Simulation


<img width="1918" height="555" alt="image" src="https://github.com/user-attachments/assets/0c49a358-328a-479f-916a-db12ec504bdf" />


### Waveform Observations
* **Input Signal ($V_{in}$, Red Trace):** A sinusoidal input with an amplitude of **6V** peak (**12V** peak-to-peak).
* **Output Signal ($V_{out2}$, Green Trace):** The resulting output waveform. With a designed voltage gain ($A_v$) of **5**, the theoretical output should reach **±30V** (**60V** peak-to-peak).

### Saturation and Clipping
Because the required output voltage significantly exceeds the operational amplifier's power supply rails ($V_{CC} =$ **±15V**), the op-amp is driven deep into saturation. The output waveform exhibits severe clipping, transforming the expected sine wave into a flat-topped waveform.

**Cursor Measurement Data:**
As indicated by the simulation cursor window, the practical saturation limits of this specific op-amp model are slightly below the supply rails due to internal voltage drops:
* Positive Saturation ($+V_{sat}$): **+14.80V**
* Negative Saturation ($-V_{sat}$): **-14.80V**

### Conclusion
The simulation successfully demonstrates the voltage transfer characteristics and the saturation limits of the op-amp. To utilize this specific configuration as a linear amplifier without distortion, the input amplitude $V_{in}$ must be restricted. 

## 5. AC Analysis (Frequency Response)

<img width="1917" height="845" alt="image" src="https://github.com/user-attachments/assets/f9ef0ef9-f9a2-453b-8d30-4944bb4a6609" />

### Calculated AC Parameters

#### 1. Mid-Band Voltage Gain ($A_{v(dB)}$)
The low-frequency, flat region of the magnitude plot represents the mid-band gain. 
$$A_{v(dB)} = 20 \log_{10}(A_v)$$
$$A_{v(dB)} = 20 \log_{10}(5) \approx 13.98\text{ dB}$$

*Observation: The graph shows the low-frequency magnitude is at 3.97dB gridline, confirming the theoretical calculation.*

#### 2. Upper Cut-off Frequency ($f_H$ or $f_{-3dB}$)


<img width="1916" height="685" alt="image" src="https://github.com/user-attachments/assets/6df1081e-a7c8-41af-8ab4-1da43c8365c9" />


The $-3\text{ dB}$ frequency is the point where the gain drops by $3\text{ dB}$ from its mid-band value. This marks the end of the amplifier's usable bandwidth.
* Target $-3\text{ dB}$ level = $13.98\text{ dB} - 3\text{ dB} = 10.98\text{ dB}$
* **Cursor 1** is placed exactly at this level ($10.949\text{ dB}$). 
* **Therefore, the Cut-off Frequency ($f_c$) = $211.69\text{ kHz}$**

#### 3. Unity Gain Frequency ($f_{unity}$)
This is the frequency at which the amplifier's closed-loop gain drops to $1\text{ V/V}$, which is $0\text{ dB}$.
* **Cursor 2** is placed at the $0\text{ dB}$ crossing ($134.7\text{ mdB} \approx 0.13\text{ dB}$).
* **Therefore, $f_{unity} \approx 887.42\text{ kHz}$**

#### 4. Gain-Bandwidth Product (GBWP)
The Gain-Bandwidth Product is a constant for a given op-amp (assuming a dominant single-pole roll-off of $-20\text{ dB/decade}$). It can be calculated using the closed-loop linear gain and the $-3\text{ dB}$ cutoff frequency.
$$GBWP = A_v \times f_c$$
$$GBWP = 5 \times 211.695\text{ kHz}$$
$$GBWP = 1.058\text{ MHz}$$

## Part E: Voltage Follower (Unity-Gain Buffer)

### 1. Theory
A **Voltage Follower** (also known as a unity-gain amplifier or buffer) is an operational amplifier circuit where the output voltage exactly tracks the input voltage. This is achieved by connecting the output directly to the inverting (-) terminal, creating 100% negative feedback, while applying the input signal to the non-inverting (+) terminal.

**Key Characteristics:**
* **Voltage Gain:** The closed-loop voltage gain ($A_v$) is exactly $1\text{ V/V}$ ($0\text{ dB}$).
* **Phase:** There is a $0^\circ$ phase shift between the input and output.
* **Impedance Matching:** It possesses an extremely high input impedance (drawing virtually zero current from the signal source) and an extremely low output impedance. This makes it an ideal "buffer" circuit to prevent a low-impedance load from loading down a high-impedance source.

The general non-inverting gain formula is:
$$A_v = 1 + \frac{R_f}{R_1}$$
In a voltage follower configuration, the feedback resistor ($R_f$) is a short circuit ($0\text{ }\Omega$) and the input resistor ($R_1$) is an open circuit ($\infty\text{ }\Omega$). 
$$A_v = 1 + \frac{0}{\infty} = 1\text{ V/V}$$

---

### 2. Design Calculations

#### Given Specifications:
* Supply Voltage: $V_{CC} = \pm 15\text{ V}$
* Input Voltage: $V_{in(pp)} = 12\text{ V}$ ($V_{in(peak)} = 6\text{ V}$)
* Input Frequency: $f = 2\text{ kHz}$
* Target Voltage Gain: $A_v = 1\text{ V/V}$
* Load Resistance: $R_L = 2.2\text{ k}\Omega$

#### Step 1: Component Selection
Unlike other amplifier configurations, the voltage follower requires no external resistors for setting the gain.
* **Feedback connection:** A direct wire from the output pin to the inverting input pin.

#### Step 2: Output Voltage Calculation
Calculate the expected peak-to-peak output.
$$V_{out(pp)} = A_v \times V_{in(pp)}$$
$$V_{out(pp)} = 1 \times 12\text{ V} = 12\text{ V}$$

**Saturation Check:** The expected output peak is $6\text{ V}$. Since $6\text{ V}$ is well below the op-amp's saturation limits ($V_{sat} \approx \pm 14\text{ V}$ for a $\pm 15\text{ V}$ supply), the output will be completely linear and undistorted, perfectly matching the input.

#### Step 3: Load Current Calculation
To ensure the op-amp can drive the specified load without exceeding its output current limits:
$$I_{L(peak)} = \frac{V_{out(peak)}}{R_L}$$
$$I_{L(peak)} = \frac{6\text{ V}}{2.2\text{ k}\Omega} \approx 2.73\text{ mA}$$
Typical standard op-amps can source/sink around $20\text{ mA}$, so a $2.73\text{ mA}$ load is well within safe operating limits.

#### Step 4: Summary Table

| Parameter | Value |
| :--- | :--- |
| **Gain ($A_v$)** | $1\text{ V/V}$ |
| **Feedback Resistor ($R_f$)** | $0\text{ }\Omega$ (Direct Short) |
| **Input Resistor ($R_1$)** | $\infty\text{ }\Omega$ (Not Used) |
| **Theoretical $V_{out(pp)}$** | $12\text{ V}$ |
| **Peak Load Current ($I_L$)** | $2.73\text{ mA}$ |

---

### 3. Expected Waveforms
The transient analysis will show two perfectly overlapping sine waves. The green output trace ($V_{out}$) will be identical in amplitude ($12\text{ V}$ peak-to-peak) and phase to the red input trace ($V_{in}$), with no clipping or distortion.

