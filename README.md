# EAHE-Soil-Temperature-Forecasting
# Evaluating Forecasting Models for Soil Temperature Dynamics in EAHE Systems

A time-series forecasting research project investigating soil temperature dynamics at multiple depths in an Earth-to-Air Heat Exchanger (EAHE) system.

## Overview

Earth-to-Air Heat Exchangers use the relatively stable temperature of the ground to exchange heat with incoming air. Predicting soil temperature at different depths can help researchers understand subsurface thermal patterns and investigate forecasting approaches for energy-efficient building systems.
<img width="840" height="397" alt="image" src="https://github.com/user-attachments/assets/f7305a16-7944-4dae-894e-5193f93362c6" />


This project evaluates conventional forecasting methods, machine-learning models, ensemble stacking, and a Physics-Informed Neural Network (PINN) approach for soil temperature prediction.

## Objectives

- Analyze soil-temperature patterns across multiple depths and seasons.
- Compare forecasting models using RMSE, MAE, and MAPE.
- Investigate stacking ensembles to combine model predictions.
- Explore physics-informed learning for soil-temperature modelling.

## Methods

The project presentation describes the following modelling approaches:

- Dynamic Mode Decomposition (DMD)
- Dynamic Mode Decomposition with control (DMDc)
- ARIMAX
- Support Vector Regression (SVR)
- XGBoost
- Stacking ensembles
- Physics-Informed Neural Networks (PINNs)
<img width="1294" height="744" alt="image" src="https://github.com/user-attachments/assets/9c913e66-958c-4ea3-84b8-8708083c1195" />

## Dataset

The original dataset contains time-series measurements collected between August 2022 and July 2025. The research presentation reports 101,399 data points and an 80:20 training/testing split.

**Dataset access is restricted.** The original measurements are not included in this repository. Access to the data is required to reproduce the experiments.
<img width="766" height="527" alt="image" src="https://github.com/user-attachments/assets/0b32b185-4ab6-409c-9d89-885a8ec287e3" />

## Evaluation

The experiments use RMSE, MAE, and MAPE to compare model predictions. Results and conclusions will be documented after the notebook outputs and evaluation methodology have been reviewed for consistency.

## Repository Contents

- `notebooks/` — Sanitized research notebooks, subject to data-sharing permissions.
- `results/` — Approved, non-sensitive plots and result summaries, if permitted.
- `requirements.txt` — Python dependencies.
- `.gitignore` — Excludes local data and generated files.



### 1. Baseline Forecasting Models

The initial experiments compare five approaches:

- **Dynamic Mode Decomposition (DMD):** Models temporal dynamics using a data-driven dynamical-system representation.
- **Dynamic Mode Decomposition with control (DMDc):** Extends DMD to incorporate control or external input information.
- **ARIMAX:** A statistical time-series model incorporating autoregressive terms and exogenous variables.
- **Support Vector Regression (SVR):** Models nonlinear relationships using a kernel-based regression approach.
- **XGBoost:** A gradient-boosted tree model for learning nonlinear relationships in the input features.

The baseline models are evaluated using RMSE, MAE, MAPE, and, where reported, R² and error variability.

### 2. Stacking Ensemble Models

The project investigates stacking to combine the predictive strengths of different base models. The base-model predictions are passed to a second-stage meta-learner, which produces the final forecast.

Two meta-learner strategies are explored:

- **MLP meta-learner:** A multilayer perceptron learns nonlinear combinations of the base-model predictions.
- **ARIMAX meta-learner:** An autoregressive model uses base-model predictions as exogenous inputs, allowing the second stage to model temporal dependencies.
<img width="1054" height="738" alt="image" src="https://github.com/user-attachments/assets/1e709fc4-a3ad-47ee-b8ac-eed6d3299302" />

The reported combinations include:

| Base-model combination | Meta-learner |
|---|---|
| DMDc + DMD | MLP |
| DMDc + ARIMAX | MLP |
| DMDc + SVR | MLP |
| DMDc + XGBoost | MLP |
| ARIMAX + ARIMAX | MLP |
| DMDc + XGBoost | ARIMAX |
| ARIMAX + ARIMAX | ARIMAX |
| DMDc + ARIMAX | ARIMAX |

The experiments compare these configurations across all three soil depths using RMSE, MAE, and MAPE.

### 3. Physics-Informed Neural Network (PINN)

A separate PINN experiment investigates the integration of data-fitting objectives and physics-based constraints into soil-temperature modelling.

The presentation reports a training configuration of 3,000 epochs, 2,000 collocation points, and L-BFGS optimization. The physics-informed approach is evaluated separately from the conventional and stacking models.
<img width="1339" height="636" alt="image" src="https://github.com/user-attachments/assets/dd1d5b34-3c09-4b89-b2f7-2d23bcf44d39" />

## Results and Discussion

### Baseline Model Findings

The final presentation reports that DMD achieved the lowest MAPE, RMSE, and MAE across the evaluated depths in the initial baseline comparison. XGBoost was also competitive and was identified as a strong baseline in the presentation's model-performance analysis.

The reported MAPE values for DMD were:

| Soil depth | DMD MAPE |
|---|---:|
| 15 cm | 2.791% |
| 25 cm | 2.259% |

At 35 cm, XGBoost achieved a reported MAPE of 2.006%. The presentation also reports that ARIMAX performed relatively poorly at 25 cm, with a MAPE of 5.583%.

These findings illustrate that model performance varies with depth and modelling assumptions.

### Stacking Results

The presentation reports improvements from stacking different model combinations. For example, the **DMDc + XGBoost → ARIMAX** configuration achieved the following reported results:

| Soil depth | RMSE | MAE | MAPE |
|---|---:|---:|---:|
| 15 cm | 0.0472 | 0.0357 | 0.118 |
| 25 cm | 0.0348 | 0.0251 | 0.0836 |
| 35 cm | 0.0413 | 0.0216 | 0.0730 |

The presentation also reports a MAPE as low as 0.069 and an R² as high as 0.96 for certain stacked configurations at Port 3. The exact configuration and metric calculation should be confirmed against the original notebook before these values are used as headline results.

### PINN Results

The presentation reports the following PINN test metrics after L-BFGS tuning:

| Port | Test MAE | Test RMSE | Test MAPE |
|---|---:|---:|---:|
| Port 1 | 0.8160 | 0.9910 | 2.73 |
| Port 2 | 0.7020 | 0.8670 | 2.37 |
| Port 3 | 0.5483 | 0.6954 | 1.84 |

The PINN results are reported separately from the stacking experiments. Their metrics should not be directly compared with the stacking results until the target scaling, units, and MAPE calculation are confirmed to be consistent.

## Interpretation

The experiments investigate three complementary approaches to forecasting soil temperature:

1. **Dynamical-system and statistical models** provide interpretable baselines for temporal behaviour.
2. **Machine learning and stacking** investigate nonlinear prediction and combinations of different model outputs.
3. **Physics-informed learning** explores whether physical constraints can complement data-driven prediction.

The reported findings are specific to the experimental dataset and evaluation procedure. They do not, by themselves, establish performance on other EAHE systems or deployment readiness.
## Reproducibility and Data Privacy

The original dataset and restricted project materials must not be redistributed. Notebook code, outputs, metadata, file paths, and embedded data must be reviewed before any public release.

Reproducing the experiments requires authorized access to the dataset and a compatible Python environment.

## Research Status

This repository documents experimental research. Reported model performance should be interpreted in the context of the original dataset and evaluation protocol; results do not establish generalization to other EAHE installations.

## Acknowledgements

The project was completed as part of academic research. Any institutional or collaborator acknowledgements will be included in accordance with the applicable permissions.
