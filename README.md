# KAVACH — ICS Anomaly Detection on BATADAL

> Detecting cyber-attacks on Industrial Control Systems using Machine Learning over SCADA time-series data from a simulated water distribution network.

---

## Problem Statement

Industrial Control Systems (ICS) are critical infrastructure — water treatment, power grids, pipelines — and increasingly the target of sophisticated cyber-attacks. SCADA systems collect real-time field measurements from PLCs (Programmable Logic Controllers) across these networks. The challenge: distinguishing a genuine attack (e.g. a pump manipulation causing a tank overflow) from normal operating variance in noisy, multivariate time-series data.

This project addresses that problem using the **BATADAL** (Battle of the Attack Detection Algorithms) dataset.

---

## Dataset

The BATADAL dataset models **C-Town**, a simulated water distribution network with 388 nodes, 429 pipes, and 5 District Metered Areas (DMAs).

### SCADA Features (43 columns + label)

| Feature Group | Variables | Count |
|---|---|---|
| Tank water levels | T1 – T7 | 7 |
| Pump status & flow | PU1 – PU11 | 11 |
| Actuated valve | V2 | 1 |
| Inlet/outlet pressures | 24 pipe measurement points | 24 |

**Label column:** `ATT_FLAG` — `1` = system under attack, `0` = normal operation.

### Splits

| Dataset | Duration | Samples | Attacks |
|---|---|---|---|
| Training Set 1 | ~1 year | 8,761 | None (pure normal data) |
| Training Set 2 | ~6 months | 4,177 | Yes (partially labeled) |
| Test Set | ~3 months | — | Yes (no labels; used for benchmarking) |

Training Set 1 was released **November 20, 2016**. Training Set 2 followed on **November 28, 2016**. The test set was released **February 20, 2017**.
---

## Repository Structure

```
KAVACH/
├── data/
│   ├── train_dataset_1.csv      # Normal operations, ~1 year
│   ├── train_dataset_2.csv      # Mixed, partially labeled
│   └── test_dataset.csv         # Unlabeled test set
├── notebooks/              
│   ├──isolation_forest.ipynb # Baseline model + failure diagnosis
├── logs/
│   └── session_log.md           # Documented decisions and findings
└── README.md
```

---

## Progress Log

Changes, experimental results, and model iterations are documented in [`logs/session_log.md`](logs/session_log.md).

---

## References

- [BATADAL Competition](http://www.batadal.net/) — Taormina et al., 2018
- IEC 62443 — Industrial Cybersecurity Standard
- Isolation Forest — Liu et al., 2008
