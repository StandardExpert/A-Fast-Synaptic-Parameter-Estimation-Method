# A Fast Synaptic Parameter Estimation Method  
### Fast Calculation Scheme Based on Second-Order Moments

---

## Overview

This repository provides the MATLAB implementation of the fast synaptic parameter estimation method described in:

> **Fast calculation scheme based on second-order moments**

The project implements a rapid and analytically grounded approach to estimate synaptic parameters — including quantal size (q), number of release sites (N), and release probability (p_i) — from EPSC train recordings.

The method leverages:

- First- and second-order statistics (mean, variance, covariance)
- Monte Carlo validation
- Markram–Tsodyks (TM) dynamic synapse model
- Optional TM-based calibration for improved robustness

This repository includes:

- Simulation of EPSC trains using a dynamic synapse model
- Parameter inversion using moment-based estimation
- Monte Carlo validation framework
- Full reproduction code for all manuscript figures

---

## Repository Structure

```
.
├── run_demo.m                       # Main entry script (figure generation)
├── Functions/
│   ├── GenerateSimuEPSC.m           # Markram–Tsodyks synapse simulator
│   ├── SimulateEPSCPeaks.m          # Multi-sweep EPSC peak simulation
│   ├── estimate_synapse_params.m    # Core second-order moment inversion
│   ├── fit_TM_from_epsc.m           # TM model fitting
│   ├── simulate_TM_phys.m           # TM forward model
│   ├── ValidateFastScheme.m         # Monte Carlo validation framework
│   ├── GetDemoCellParams.m          # Default demo parameters
│   ├── GetColor.m                   # Figure color utilities
│   ├── saveFigureTransparent.m      # Transparent export utility
│   ├── parsave.m                    # Parallel-safe saving
│   ├── SimuEPSCUtils.m              # Simulation visualization helper
│   ├── SimulateEPSCPeaks.m          # EPSC peak statistics
│   └── abfload.m                    # ABF file loader (Axon format)
```

---

## Quick Start

### 1. Requirements

- MATLAB R2021a or later
- Statistics and Machine Learning Toolbox (for `ksdensity`, `rmoutliers`)
- Parallel Computing Toolbox (optional, for `parfor` acceleration)

---

### 2. Run Demo (Generate All Figures)

Simply execute:

```matlab
run_demo
```

This script:

- Generates all figures used in the manuscript
- Runs simulation and Monte Carlo validation
- Demonstrates robustness analysis under:
  - Measurement noise
  - Trial-to-trial quantal variability
  - Sweep number changes
  - TM calibration effects

All figures are exported as vector graphics.

---

## Core Method

### 1. Simulation

EPSC trains are generated using a stochastic Markram–Tsodyks dynamic synapse model:

- Release probability:  
  p_i = R_i · u_i
- Vesicle release:  
  n_i ~ Binomial(N, p_i)
- EPSC amplitude:  
  EPSC_i = q · n_i + noise

Implemented in:

```
Functions/GenerateSimuEPSC.m
Functions/SimulateEPSCPeaks.m
```

---

### 2. Fast Parameter Estimation (Second-Order Moment Method)

The proposed method estimates:

- q (quantal size)
- N (number of release sites)
- p_i (release probability sequence)

Using:

- Mean EPSC
- Variance across sweeps
- Covariance between stimuli

Core implementation:

```
Functions/estimate_synapse_params.m
```

This avoids full likelihood fitting and dramatically reduces computational complexity.

---

### 3. TM-Based Calibration (Optional)

To improve robustness in noisy regimes, we optionally fit a TM model:

```
Functions/fit_TM_from_epsc.m
Functions/simulate_TM_phys.m
```

Calibration relationship:

```
EPSC_mean = N * q * p_i = A * p_i
⇒ N = A / q
```

---

## Validation

### Monte Carlo Validation

Implemented in:

```
Functions/ValidateFastScheme.m
```

Procedure:

1. Randomly sample synaptic parameters
2. Simulate EPSC trains
3. Recover parameters using moment inversion
4. Compare true vs estimated distributions

Outputs include:

- Bias
- RMSE
- Parameter scatter plots
- Distribution comparisons

---

## Reproducing Manuscript Figures

All figures are generated within:

```
run_demo.m
```

Figures include:

| Figure | Description |
|--------|-------------|
| Fig 2  | Variance–Covariance schematic |
| Fig 3  | EPSC statistics & covariance |
| Fig 4  | Monte Carlo parameter distributions |
| Fig 5  | Robustness analysis under noise and variability |

---

## Adjustable Parameters

Default demo parameters are defined in:

```
Functions/GetDemoCellParams.m
```

Example:

```matlab
params.q          = 10;
params.N          = 100;
params.U          = 0.05;
params.tau_rec    = 300;
params.tau_facil  = 1500;
params.stim_freq  = 50;
params.num_stim   = 10;
params.noise_std  = 5;
params.num_sweeps = 30;
```

---

## Performance Characteristics

Compared to conventional fitting approaches:

- No nonlinear global optimization required
- Extremely fast (analytic inversion)
- Stable under moderate noise
- Easily parallelizable

---

## Data Compatibility

The repository includes:

```
Functions/abfload.m
```

for loading Axon ABF electrophysiology files.

You may replace simulated EPSC data with real recordings and directly apply:

```matlab
estimate_synapse_params(data, noise_std)
```

---

## License

This project is provided for academic research use.

Please contact the authors for licensing details if used commercially.

---

## Contact

For questions or collaboration:

[Liber T. Hua]  
[Beijing Normal University]  
[202531061015@mail.bnu.edu.cn]

---

## Summary

This repository provides:

- A fast analytic synaptic parameter estimator
- Full Monte Carlo validation
- TM-model consistency checks
- Publication-ready visualization tools

It enables rapid and scalable analysis of short-term synaptic plasticity data using second-order statistical structure.

---
