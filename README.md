# Semi-Active LQR Suspension & Braking Co-Simulation
### 2-DOF Half-Car Model | MATLAB/Simulink | Öhlins-Inspired Damper Constraints

This repository contains a MATLAB/Simulink modeling framework for evaluating a semi-active Linear Quadratic Regulator (LQR) suspension controller during transient braking maneuvers. The project investigates whether semi-active damping control can reduce vehicle body motion compared with a passive suspension baseline, subject to realistic damper constraints.

The work forms part of a vehicle dynamics and control research project focused on semi-active suspension systems, transient braking response, and Öhlins damper actuation characteristics.

---

## 🚗 Project Overview & Objectives

The primary objective is to investigate the performance of a **semi-active LQR-controlled suspension** relative to a passive baseline during transient braking, with particular emphasis on:

- **Transient Pitch and Heave Suppression:** Reducing body bounce, braking-induced pitch, and subsequent oscillations.
- **Tire-Road Interaction:** Investigating the effects of suspension dynamics and longitudinal load transfer on tire normal loads and available braking forces under low-friction conditions.
- **Realistic Damper Actuation:** Incorporating damping coefficient bounds, passivity constraints, and valve response-rate limitations inspired by Öhlins semi-active damper characteristics.

The current simulation evaluates body-motion performance under a low-friction braking condition of **μ = 0.4**.

## 📐 System Architecture & Modeling

### 1. Plant Dynamics — 2-DOF Half-Car Model

The vehicle body is represented by a two-degree-of-freedom model capturing:

- **Heave** (\(z\)): Vertical translation of the sprung mass.
- **Pitch** (\(\theta\)): Angular rotation of the sprung mass about its center of gravity.

The suspension inputs are the front and rear damping forces, \(F_{df}\) and \(F_{dr}\), together with the effects of braking-induced pitch moments.

### 2. Semi-Active LQR Controller

An LQR controller is used to regulate the vehicle's body response by determining appropriate suspension control actions.

The controller performance is evaluated against a passive suspension baseline, focusing on peak heave displacement, heave velocity, pitch angle, and pitch angular velocity during transient braking.

### 3. Öhlins-Inspired Damper Actuation Constraints

The damper realization subsystem incorporates the following constraints:

- **Damping coefficient saturation:** \(c_{\min}=500\ \text{N·s/m}\) and \(c_{\max}=4000\ \text{N·s/m}\).
- **Passivity constraint:** Velocity-guard logic enforces dissipative damper behavior, expressed as \(F_d\dot{x}_{rel}\leq 0\), subject to the chosen force and velocity sign conventions.
- **Valve response-rate limitation:** A rate limiter approximates finite solenoid response speed.

These constraints help bridge the gap between idealized control commands and physically realizable semi-active damping behavior. They represent modeled actuator limitations, not a validated model of a specific Öhlins product.

### 4. Nonlinear Tire Friction and Load Transfer — Tier 2

The extended model calculates front and rear normal loads, accounting for static weight distribution, suspension dynamics, and longitudinal load transfer.

The available longitudinal tire force is bounded by the friction limit:

\[
|F_x| \leq \mu F_z
\]

where \(\mu\) is the tire-road friction coefficient and \(F_z\) is the tire normal load.

This subsystem provides a basis for investigating how suspension dynamics interact with braking-force limitations. Improvements in body-motion response alone do not necessarily imply shorter braking distances.

---

## 📊 Simulation Results

### Quantitative Comparison — Passive vs. Semi-Active LQR

The preliminary simulation results at **μ = 0.4** show reductions in the peak magnitudes of all four evaluated body-motion metrics when using semi-active LQR control.

| Kinematic metric | Passive baseline | Semi-active LQR | Peak reduction |
|:--|--:|--:|--:|
| Heave displacement, \(z\) [m] | 0.00859 | 0.00569 | 33.77% |
| Heave velocity, \(\dot{z}\) [m/s] | 0.02645 | 0.01152 | 56.44% |
| Pitch angle, \(\theta\) [rad] | 0.03263 | 0.02685 | 17.72% |
| Pitch angular velocity, \(\dot{\theta}\) [rad/s] | 0.11981 | 0.08293 | 30.78% |

*Peak reduction is calculated relative to the passive baseline. The values represent reductions in peak magnitudes, not necessarily reductions at every instant of the simulation.*

### Key Findings

- **Heave velocity reduced by 56.44%:** The largest relative reduction among the four evaluated metrics, indicating improved suppression of vertical body motion.
- **Heave displacement reduced by 33.77%:** The semi-active controller reduces the peak vertical displacement of the vehicle body.
- **Pitch angular velocity reduced by 30.78%:** The controller mitigates the rate of braking-induced body rotation.
- **Pitch angle reduced by 17.72%:** The peak body pitch response is lower than that of the passive baseline.

Overall, these preliminary results indicate that semi-active LQR control can improve transient body-motion behavior in the modeled low-friction braking scenario.

These findings establish improvements in the evaluated kinematic metrics; further analysis of tire normal loads, braking forces, and stopping distance is required to determine the effect on overall braking performance.

### Heave Response

![Heave response comparison](acc_result.png)

### Pitch Response

![Pitch response comparison](result1.png)

---

## 📁 Repository Structure

```text
├── tire_brake_deceleration.slx  # Main 2-DOF Simulink co-simulation model
├── init_parameters.m            # Model parameters, state-space matrices, and LQR gains
├── Post_processing.m            # Post-processing and performance metrics
├── subsystems/                  # Damper and normal-load calculation subsystems
├── acc_result.png               # Heave-related simulation figure
├── result1.png                  # Pitch-related simulation figure
└── README.md                    # Project documentation
```

*The file and folder names above should match the actual repository structure. Update the figure paths or descriptions if the files are stored elsewhere or represent different outputs.*

## 🛠️ Tools & Methods

- **MATLAB** — Vehicle dynamics calculations, controller design, and post-processing.
- **Simulink** — Dynamic system modeling and braking/suspension co-simulation.
- **Linear Quadratic Regulator (LQR)** — State-feedback control design.
- **State-space modeling** — Representation of coupled heave and pitch dynamics.
- **Semi-active actuator constraints** — Damping saturation, passivity guards, and response-rate limitations.
- **Tire friction modeling** — Friction-limited braking force and dynamic normal-load analysis.

## 🔬 Future Work

Potential extensions to the current framework include:

- Quantifying braking distance and stopping-time differences between passive and semi-active configurations.
- Evaluating dynamic front and rear tire normal loads and tire-force utilization.
- Investigating the sensitivity of controller performance to road friction, vehicle speed, and braking intensity.
- Extending the model to include additional suspension and wheel dynamics.
- Evaluating controller robustness under actuator bandwidth limitations and varying road conditions.
- Validating the simulation against a higher-fidelity vehicle model or experimental data.

## ⚠️ Scope & Limitations

The reported results are preliminary simulation findings for a 2-DOF half-car model under a specified low-friction braking condition. They should not be interpreted as experimental validation or as evidence of a specific production damper's performance.

The reported reductions apply to the evaluated peak kinematic metrics. Braking-distance improvements, tire-force utilization benefits, and hardware-level performance require separate quantitative verification.

---

**Project focus:** Vehicle dynamics · Optimal control · Semi-active suspension · Braking performance · Mechatronics
