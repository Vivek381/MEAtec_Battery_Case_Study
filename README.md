# MEAtec Case Study
## LFP Cell Physical Modeling & Parametrization in PyBaMM

This repository contains the implementation, parameter analysis, optimization, and validation of a Doyle–Fuller–Newman (DFN) model for the A123 ANR26650M1B lithium iron phosphate (LFP) cell as part of the MEAtec case study.

---

## 1. Cell and Model

- **Cell:** A123 ANR26650M1B / Lithium Werks
- **Chemistry:** Lithium Iron Phosphate (LFP)
- **Nominal capacity:** 2.5 Ah
- **Electrochemical model:** Doyle–Fuller–Newman (DFN)
- **Initial parameter set:** Prada2013
- **Simulation framework:** PyBaMM 26.8.0.0

---

## 2. Objectives

The main objectives of the case study are to:

1. Implement a DFN model using an open-source parameter set.
2. Evaluate the model against experimental HPPC data.
3. Analyze voltage residuals and identify important model parameters.
4. Perform one-factor-at-a-time parameter sensitivity analysis.
5. Optimize selected parameters using a bounded numerical optimization method.
6. Validate the optimized DFN model against the available experimental datasets.
7. Analyze relevant physical variables during HPPC operation.

---

## 3. Experimental Data

The case study includes the following experimental datasets:

- Charge 1C
- Charge 2C
- Charge 3C
- Charge 4C
- Discharge 1C
- HPPC

Temperature effects were not treated as an optimization dimension because no temperature range was specified in the case-study data.

Raw experimental data are not included in this repository where redistribution is restricted. Authorized users should place the required data files in the appropriate local data directory.

---

## 4. Methodology

The workflow used in this study consists of the following steps:

### 4.1 DFN Model

A Doyle–Fuller–Newman model was implemented in PyBaMM using the Prada2013 parameter set as the initial parameterization.

### 4.2 HPPC Evaluation

The experimental HPPC current profile was supplied directly to the DFN model. Simulated terminal voltage was compared with the experimental voltage.


### 4.3 Residual Analysis

The residual response was analyzed to identify:

- Systematic voltage offsets
- Current-dependent transient errors
- Kinetic effects
- Electrolyte transport effects

### 4.4 Sensitivity Analysis

A simple one-factor-at-a-time sensitivity analysis was performed by varying selected parameters individually and evaluating the resulting HPPC voltage error.

### 4.5 Parameter Optimization

Four parameters were selected for optimization:

- Positive-electrode OCP shift
- Negative-electrode exchange-current density
- Electrolyte conductivity
- Electrolyte diffusivity

The optimization was performed using the **L-BFGS-B** algorithm with bounded continuous parameters.

The objective function was defined as the mean normalized RMSE across:

- Charge 1C
- Charge 2C
- Charge 3C
- Charge 4C
- HPPC

### 4.6 Validation

The optimized parameter set was used to reconstruct the DFN model and evaluate the voltage response. Additional physical variables were analyzed, including:

- Negative-electrode equilibrium potential
- Negative-electrode reaction overpotential
- Negative-particle surface stoichiometry

---

## 5. Optimized Parameter Set

The final optimized parameter set obtained from the optimization workflow is:

| Parameter | Optimized value |
|---|---:|
| Positive-electrode OCP shift | 25.15325 mV |
| Negative-electrode exchange-current density multiplier | 3.53854× |
| Electrolyte conductivity multiplier | 0.20× |
| Electrolyte diffusivity multiplier | 1.06384× |

The electrolyte-conductivity multiplier reached its lower optimization bound. A fine sensitivity scan around this region was subsequently performed, with the minimum mean RMSE among the tested values occurring at 0.20×.

---

## 6. Results

The optimized parameter set reduced the mean RMSE across the four charge datasets and HPPC from:

**50.789 mV → 29.167 mV**

This corresponds to an approximately **42.57% reduction** in the mean RMSE.

The final HPPC validation metrics were:

| Metric | Value |
|---|---:|
| RMSE | 19.313 mV |
| MAE | 14.182 mV |
| Maximum absolute error | 91.296 mV |
| Mean residual | 0.688 mV |
| Residual standard deviation | 19.301 mV |

The optimized model provides improved agreement with the available experimental voltage response, while some transient discrepancies remain during high-current HPPC pulses.


