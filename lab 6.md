# LAB-06: Design the Circuit Using Op-Amp 

## 1.Theory 

## Introduction
Operational Amplifiers (Op-Amps) are high-gain electronic voltage amplifiers with differential inputs and, usually, a single-ended output. In this experiment, the Op-Amp is used as a **Linear Mathematical Element**. By utilizing negative feedback, the closed-loop gain of the circuit is determined almost entirely by external components (resistors), independent of the Op-Amp's internal high open-loop gain.

## Inverting Summing Amplifier (Part A)
The inverting summer is an Op-Amp circuit that can add multiple input signals, each weighted by a specific gain. 

### Working Principle
Based on the **Virtual Ground** concept, the inverting terminal $(-)$ is maintained at approximately $0\text{V}$ because the non-inverting terminal $(+)$ is grounded. According to Kirchhoff’s Current Law (KCL), the sum of currents entering the summing node must equal the current flowing through the feedback resistor:
$$I_{f} = I_{1} + I_{2} + ... + I_{n}$$

Substituting Ohm's Law ($I = V/R$):
$$\frac{0 - V_{out}}{R_f} = \frac{V_1 - 0}{R_1} + \frac{V_2 - 0}{R_2}$$

This results in the transfer function:
$$V_{out} = -\left( \frac{R_f}{R_1}V_1 + \frac{R_f}{R_2}V_2 \right)$$

## Difference Amplifier / Subtractor (Part B)
A difference amplifier amplifies the voltage difference between two input signals while rejecting any common-mode signal.

### Working Principle
This circuit combines an inverting and a non-inverting configuration. 
1. The voltage at the non-inverting terminal ($V_+$) is determined by a voltage divider:
   $$V_+ = V_{in1} \left( \frac{R_4}{R_3 + R_4} \right)$$
2. Due to **Virtual Short** behavior, the Op-Amp forces $V_- = V_+$.
3. The output is then derived by considering the gain applied to this $V_+$ value minus the inverting path contribution.

The general transfer function is:
$$V_{out} = \left( \frac{R_4}{R_3 + R_4} \right) \left( 1 + \frac{R_f}{R_{in}} \right) V_{in1} - \left( \frac{R_f}{R_{in}} \right) V_{in2}$$

For a standard subtractor where $R_{in} = R_3$ and $R_f = R_4$, the equation simplifies to $V_{out} = \frac{R_f}{R_{in}}(V_{in1} - V_{in2})$. However, by varying these ratios, we can achieve specific coefficients for each input.

## Simulation Specifications: 

### Input Signal Definitions
* **x1(t):** +0.5V DC
* **x2(t):** +0.5V DC
* **x3(t):** 0.5 sin(3000πt) [Frequency = 1.5 kHz]
* **Supply:** ±12V

### Simulation Tasks
| Part | Functional Requirement | Circuit Implementation |
| :--- | :--- | :--- |
| **a** | y1(t) = -[5x1(t) + x2(t)] | Inverting Summing Amp |
| **b** | y2(t) = x1(t) - 3x2(t) | Differential / Subtractor Amp |


## 2. Design circuit A: Inverting Summer
**Target Equation:** $y_1(t) = -[5x_1(t) + x_2(t)]$


### Design Methodology
An **Inverting Summing Amplifier** topology is used. The output is governed by:
$$V_{out} = -\left( \frac{R_f}{R_1}x_1 + \frac{R_f}{R_2}x_2 \right)$$

### Component Selection
- **Feedback Resistor ($R_f$):** $10\text{ k}\Omega$
- **Input Resistor for $x_1$ ($R_1$):** $2\text{ k}\Omega$ (Calculated as $10\text{k} / 5$)
- **Input Resistor for $x_2$ ($R_2$):** $10\text{ k}\Omega$ (Calculated as $10\text{k} / 1$)

  
<img width="1020" height="492" alt="image" src="https://github.com/user-attachments/assets/fb0d8162-4956-48bf-a5d3-9eae2db4d998" />


---

## Technical Specifications
- **Op-Amp:** LM741 / TL081 (or equivalent)
- **Supply Voltage ($V_{CC}$):** $\pm 12\text{V}$
- **Input Conditions:** $x_1 = +0.5\text{V}$, $x_2 = +0.5\text{V}$

### Expected Simulation Results
| Task | Calculated Output |
| :--- | :--- |
| **Part (a)** | $-3.0\text{V}$ |


## 3.DC Analysis

<img width="1505" height="856" alt="image" src="https://github.com/user-attachments/assets/ffb00391-3528-4930-8eaf-061872d0cbb2" />

##  Simulation Results: DC Operating Point Analysis (Part A)

The circuit for ($y_1(t) = -[5x_1(t) + x_2(t)]$) was simulated using LTspice with a $\mu A741$ Op-Amp model in an **Inverting Summing Amplifier** configuration.

### Simulation Parameters
- **Inputs:** $V_x = 0.5\text{ V}$, $V_y = 0.5\text{ V}$ (labeled as V7 and V8 in schematic)
- **Supply:** $\pm 12\text{ V}$
- **Command:** `.op`

### Results Interpretation

| Parameter | Simulated Value | Theoretical Value | Deviation |
| :--- | :--- | :--- | :--- |
| **V(vout)** | $-2.99896\text{ V}$ | $-3.00\text{ V}$ | $0.034\%$ |
| **V(n001)** | $34.5\ \mu\text{V}$ | $0\text{ V}$ (Virtual Ground) | Negligible |

#### 1. Output Voltage Accuracy
The mathematical design for Part A is:
$$V_{out} = -\left( \frac{R_5}{R_7}V_y + \frac{R_5}{R_6}V_x \right)$$
$$V_{out} = -\left( \frac{10\text{k}}{2\text{k}}(0.5) + \frac{10\text{k}}{10\text{k}}(0.5) \right) = -(2.5 + 0.5) = -3.0\text{ V}$$
The simulated value of **$-2.99896\text{ V}$** confirms the resistor ratios ($5:1$) are correctly weighting the inputs.

#### 2. Virtual Ground Verification
Node `V(n001)` represents the inverting terminal input. In an ideal inverting amplifier, this should be at $0\text{ V}$ (Virtual Ground).
- **Simulated Node Voltage:** $34.5028\ \mu\text{V}$
This extremely low voltage (in the microvolt range) validates that the negative feedback is working correctly, maintaining the inverting terminal very close to ground potential.

#### 3. Current Distribution Analysis
- **Input Current $I(R_7)$:** $0.25\text{ mA}$ (Calculated: $0.5\text{ V}/2\text{k} = 0.25\text{ mA}$)
- **Input Current $I(R_6)$:** $0.05\text{ mA}$ (Calculated: $0.5\text{ V}/10\text{k} = 0.05\text{ mA}$)
- **Feedback Current $I(R_5)$:** $\approx 0.30\text{ mA}$ 
The simulation shows $I(R_5) = I(R_7) + I(R_6)$, satisfying Kirchhoff’s Current Law (KCL) at the summing node.

---
###  Conclusion
The DC analysis for Part A proves that the inverting summer configuration is highly accurate. The minor microvolt offset at the summing node is attributed to the non-ideal characteristics (input offset voltage and bias current) of the $\mu A741$ model.

## 4.Transient Analysis

<img width="1918" height="852" alt="image" src="https://github.com/user-attachments/assets/853c9499-8f18-4bc4-bc39-ea654094a67b" />

## Transient Analysis (.tran) - Part A

A transient simulation was conducted over a **5ms** interval to verify the time-domain stability and output accuracy of the inverting summer circuit ($y_1(t) = -[5x(t) + y(t)]$).

### Waveform Interpretation

| Trace | Label | Voltage Level |
| :--- | :--- | :--- |
| **V(x)** | Green Trace | $+0.5\text{V}$ (DC Input) |
| **V(y)** | Blue Trace | $+0.5\text{V}$ (DC Input) |
| **V(vout)** | Red Trace | $-3.0\text{V}$ (Steady State) |

#### 1. Transfer Function Verification
The simulation results confirm the intended mathematical operation:
$$V_{out} = -\left( \frac{R_5}{R_6}V_x + \frac{R_5}{R_7}V_y \right)$$
$$V_{out} = -(1 \cdot 0.5\text{V} + 5 \cdot 0.5\text{V}) = -3.0\text{V}$$
The plot shows the output (red line) exactly at the **$-3.0\text{V}$** division, validating the gain coefficients for both input channels.


---
### Conclusion
The transient analysis for Part A successfully demonstrates the circuit's ability to sum multiple DC signals with specific gains. The results are consistent with the DC operating point analysis and confirm the theoretical design parameters

## 5. Design circuit B: Difference Amplifier
**Target Equation:** $y_2(t) = x_1(t) - 3x_2(t)$



[Image of op-amp difference amplifier circuit diagram]


### Design Methodology
A **Difference Amplifier** (Subtractor) is utilized. To achieve the specific gains for $x_1$ and $x_2$, the resistor network is balanced as follows:

1. **Inverting Terminal ($x_2$):** To achieve a gain of $3$, we set $\frac{R_f}{R_{in}} = 3$.
   - **$R_{in}$:** $10\text{ k}\Omega$
   - **$R_f$:** $30\text{ k}\Omega$
2. **Non-Inverting Terminal ($x_1$):** To achieve a unity gain ($1$) while accounting for the feedback ratio:
   

The coefficient for $x_1$ is 1. Let the voltage divider at the non-inverting pin be formed by $R_3$ (series) and $R_4$ (to ground).

$$1 = \frac{R_4}{R_3 + R_4} \left( 1 + \frac{30 \text{ k}\Omega}{10 \text{ k}\Omega} \right)$$

$$1 = \frac{R_4}{R_3 + R_4}(4) \implies R_3 + R_4 = 4R_4 \implies R_3 = 3R_4$$

If we choose $R_4 = 10 \text{ k}\Omega$, then $R_3 = 30 \text{ k}\Omega$.
   
   - **$R_3$ (Series):** $30\text{ k}\Omega$
   - **$R_4$ (Shunt):** $10\text{ k}\Omega$

     
<img width="1067" height="617" alt="image" src="https://github.com/user-attachments/assets/7f766030-1b05-4e1e-8fb1-1623341e9634" />
 

---

### Expected Simulation Results
| Task | Calculated Output |
| :--- | :--- |
| **Part (b)** | $-1.0\text{V}$ |


### 5.DC Operating Point Analysis (Part B)

<img width="1537" height="655" alt="image" src="https://github.com/user-attachments/assets/e96cdfe1-af7a-4ef0-8475-af11dbe638c5" />

The circuit for Task B ($y_2(t) = x_1(t) - 3x_2(t)$) was simulated using LTspice with a $\mu A741$ Op-Amp model. 

### Simulation Parameters
- **Inputs:** $V_{in1} = 0.5V$, $V_{in2} = 0.5V$
- **Supply:** $\pm 12V$
- **Command:** `.op` (DC Operating Point)

### Results Interpretation

| Parameter | Simulated Value | Theoretical Value | Deviation |
| :--- | :--- | :--- | :--- |
| **V(vout2)** | $-0.999917\text{ V}$ | $-1.00\text{ V}$ | $0.0083\%$ |
| **V(n003)** | $124.4\text{ mV}$ | $125.0\text{ mV}$ | $0.48\%$ |
| **V(n001)** | $124.4\text{ mV}$ | $125.0\text{ mV}$ | $0.48\%$ |

#### 1. Output Voltage Accuracy
The target equation for the given inputs is:
$$V_{out} = 0.5V - 3(0.5V) = -1.0V$$
The simulated output of **$-0.999917V$** 

#### 2. Virtual Short Verification
In an ideal op-amp, the voltages at the inverting and non-inverting terminals should be equal ($V_+ = V_-$). 
- **Non-Inverting Node (V_n003):** $0.1244V$ (Calculated via voltage divider: $0.5V \times \frac{10k}{10k+30k} = 0.125V$).
- **Inverting Node (V_n001):** $0.1244V$.
The negligible difference confirms the op-amp is operating correctly within its linear region and maintaining the virtual ground/short principle.

## 6.Transient Analysis

<img width="1915" height="530" alt="image" src="https://github.com/user-attachments/assets/da64e3ec-2943-40aa-8f95-1ab6cbfd8d64" />

## Transient Analysis (.tran)

The transient analysis was performed over a **5ms** duration to observe the circuit's behavior over time and ensure stability.

### Waveform Interpretation

| Trace | Source | Voltage Level |
| :--- | :--- | :--- |
| **V(vin1)** | Blue Trace | $+0.5\text{V}$ (Constant) |
| **V(vin2)** | Green Trace | $+0.5\text{V}$ (Constant) |
| **V(vout2)** | Red Trace | $-1.0\text{V}$ (Steady State) |



#### 2. Signal Verification
The plot visually verifies the difference operation:
$$V_{out}(t) = V_{in1}(t) - 3V_{in2}(t)$$
$$V_{out}(t) = 0.5\text{V} - 3(0.5\text{V}) = -1.0\text{V}$$
As shown in the graph, the vertical distance between the inputs ($+0.5\text{V}$) and the output ($-1.0\text{V}$) is exactly $1.5\text{V}$, confirming the subtraction and gain factors are working as designed.


---
### Conclusion
Both the **DC Operating Point** and **Transient Analysis** successfully validate the design. The circuit accurately implements the mathematical function $y_2(t) = x_1(t) - 3x_2(t)$ with high precision and stability.

---

## 7.Inference 

The experiment successfully demonstrated the design and validation of Operational Amplifier circuits for performing specific mathematical operations: weighted summation (Inverting Summer) and subtraction (Difference Amplifier).

### 1. Design Validation
The theoretical calculations using standard Op-Amp topologies were verified through LTspice simulation. The resistor ratios $R_f/R_{in}$ accurately controlled the gain coefficients for each input variable ($x_1, x_2, x_3$).

* **Part A (Inverting Summer):** Effectively implemented $y_1(t) = -[5x_1(t) + x_2(t)]$. The simulation yielded $-2.998\text{V}$ against a theoretical $-3.0\text{V}$.
* **Part B (Difference Amplifier):** Effectively implemented $y_2(t) = x_1(t) - 3x_2(t)$. The simulation yielded $-0.999\text{V}$ against a theoretical $-1.0\text{V}$.

### 2. Practical Observations
* **Virtual Ground & Short:** The DC analysis confirmed the "Virtual Ground" at the inverting terminal for Part A and the "Virtual Short" ($V_+ \approx V_-$) for Part B, validating the core principles of negative feedback.
* **Non-Ideal Effects:** A minor deviation (in the microvolt to millivolt range) was observed between theoretical and simulated values. This is attributed to the **Input Bias Current** and **Input Offset Voltage** inherent in the $\mu A741$ bipolar op-amp model. 
* **Stability:** The Transient Analysis confirmed that both circuits are stable under DC conditions, showing no signs of oscillation or DC drift within the $5\text{ms}$ simulation window.

### 3. Summary of Results

| Experiment Part | Operation | Theoretical $V_{out}$ | Simulated $V_{out}$ | 
| :--- | :--- | :--- | :--- |
| **Task A** | Inverting Summation | $-3.00\text{V}$ | $-2.998\text{V}$ |
| **Task B** | Subtraction | $-1.00\text{V}$ | $-0.999\text{V}$ | 




