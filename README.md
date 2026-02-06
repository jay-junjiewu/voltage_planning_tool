# Voltage Compliance Planning Tool (ML-Based)

This repository contains a per-asset, data-driven voltage planning tool that evaluates whether network intervention is required and, if so, which option delivers the best cost-effectiveness under photovoltaic (PV) injections and load variability.

The tool operationalises a repeatable machine-learning workflow to quantify voltage non-compliance and near-breach risk, estimate counterfactual outcomes (what would have happened without intervention), measure avoided risk attributable to each intervention, and support evidence-based planning decisions across feeders, transformers, or zones.

---

## Key Concepts

### Near-Breach Risk
Hard voltage breaches outside statutory limits can be rare and noisy, particularly under high PV penetration. Near-breach risk is used as the primary planning signal because it responds earlier, is sensitive to PV export peaks, cloud-edge ramps, and load swings, and provides a more stable indicator of future non-compliance risk.

Hard breaches are still reported as the statutory compliance metric.

---

## What the Tool Produces (Per Asset)

- Observed compliance outcomes  
  Annualised hard breach hours and near-breach risk hours with 95% confidence intervals.

- Counterfactual Year-2 baseline  
  A machine-learning model trained on Year-1 operating conditions and applied to Year-2 conditions to estimate what would have occurred without intervention.

- Intervention benefit  
  Avoided near-breach risk in hours per year, with quantified uncertainty.

- Economic decision support  
  Capex per avoided risk-hour, benefit-versus-cost comparison, and residual risk checks against planning targets.

---

## Interventions Considered

Tap change  
An operational voltage level adjustment with very low capital cost. Highly effective at reducing persistent over- or undervoltage but with limited impact on short-term voltage variance.

STATCOM  
A capital intervention providing fast reactive power support. Effective at reducing voltage variance and PV-driven fluctuations, typically justified only where residual risk remains high after operational controls.

---

## Methodology Overview

1. Observed outcomes are computed directly from measured voltage and annualised for comparability across monitoring windows.

2. Uncertainty is quantified using day-block bootstrap resampling to account for serial correlation in voltage behaviour.

3. Counterfactual modelling is performed by training models on Year-1 relationships between operating conditions and near-breach risk, excluding voltage as a feature, and applying the models to Year-2 conditions.

4. Feature distribution drift between Year-1 and Year-2 is assessed using Population Stability Index to flag potential regime changes and assign confidence levels.

5. Decision logic requires both cost efficiency and compliance with residual risk targets to avoid recommending low-cost but insufficient options.

---

## Inputs

Each asset dataset should contain regular time-series data (e.g. 5-minute resolution) including:

- Timestamp
- Voltage measurements
- Active and reactive power import and export
- Derived net P/Q and time-based features

Additional high-frequency features such as ramp rates and rolling volatility can improve model performance under PV and load variability.

---

## Outputs

- Annualised voltage violation and near-breach risk metrics
- Counterfactual versus observed risk comparisons
- Cost-effectiveness rankings
- Benefit-versus-cost visualisations
- ROI sensitivity under uncertainty
- Confidence flags based on feature drift

---

## Intended Use

- Distribution planning and augmentation deferral
- PV-driven voltage risk assessment
- Option screening prior to detailed power-flow studies
- Fleet-level asset prioritisation

This is a planning and decision-support tool, not a real-time control or protection system.

---

## Limitations

The approach is statistical rather than physics-based and does not explicitly model feeder impedance, phase coupling, or voltage propagation. Counterfactual validity assumes structural stability between baseline and evaluation periods. Uncertainty reflects sampling variability and day-to-day behaviour rather than worst-case engineering bounds.

---

## Future Extensions

- Incorporation of electrical context features such as impedance class and distance to transformer
- Improved handling of regime shifts due to rapid PV uptake
- Integration with portfolio-level optimisation and dashboarding

