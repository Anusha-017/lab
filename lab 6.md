# LAB-06: Design the Circuit Using Op-Amp 


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

# Op-Amp Mathematical Operations Design

This repository contains the design calculations and component values for operational amplifier circuits performing specific mathematical summations and subtractions.

## 1. Design circuit A: Inverting Summer
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

## 2. Design circuit B: Difference Amplifier
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

## Technical Specifications
- **Op-Amp:** LM741 / TL081 (or equivalent)
- **Supply Voltage ($V_{CC}$):** $\pm 12\text{V}$
- **Input Conditions:** $x_1 = +0.5\text{V}$, $x_2 = +0.5\text{V}$

### Expected Simulation Results
| Task | Calculated Output |
| :--- | :--- |
| **Part (a)** | $-3.0\text{V}$ |
| **Part (b)** | $-1.0\text{V}$ |

## DC Analysis

### Simulation Results: DC Operating Point Analysis (Part B)

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

## Transient Analysis

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
### Final Conclusion
Both the **DC Operating Point** and **Transient Analysis** successfully validate the design. The circuit accurately implements the mathematical function $y_2(t) = x_1(t) - 3x_2(t)$ with high precision and stability.

---


