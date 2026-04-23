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

## 1. Design Task A: Inverting Summer
**Target Equation:** $y_1(t) = -[5x_1(t) + x_2(t)]$



### Design Methodology
An **Inverting Summing Amplifier** topology is used. The output is governed by:
$$V_{out} = -\left( \frac{R_f}{R_1}x_1 + \frac{R_f}{R_2}x_2 \right)$$

### Component Selection
- **Feedback Resistor ($R_f$):** $10\text{ k}\Omega$
- **Input Resistor for $x_1$ ($R_1$):** $2\text{ k}\Omega$ (Calculated as $10\text{k} / 5$)
- **Input Resistor for $x_2$ ($R_2$):** $10\text{ k}\Omega$ (Calculated as $10\text{k} / 1$)

---

## 2. Design Task B: Difference Amplifier
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

