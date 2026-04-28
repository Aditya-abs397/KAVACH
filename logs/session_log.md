# Session Log — KAVACH

---

## SESSION-01 — Baseline Anomaly Detection Model

**Objective:** Establish a baseline anomaly detector on Training Set 2 using an unsupervised approach, evaluate against labeled attack data, and document failure modes before moving to a temporal model.

---

### Work Done

- Data preprocessing on Training Set 2 (4,177 samples, mixed normal/attack)
- Trained Isolation Forest with `contamination=0.12`
- Evaluated on classification metrics against `ATT_FLAG` ground truth

---

### Results

| Class    | Precision | Recall | F1-Score | Support |
|----------|-----------|--------|----------|---------|
| Normal   | 0.90      | 0.86   | 0.88     | 3,685   |
| Attack   | 0.22      | 0.30   | 0.25     | 492     |
| **Accuracy** |       |        | **0.79** | 4,177   |

---

### Why F1 Is Low

**Contamination mismatch:** `contamination=0.12` was set without calibration. The model under-flags attacks, reflected in recall of 0.30 — it misses 70% of actual attacks.

**No hyperparameter tuning:** `n_estimators`, `max_samples`, and `contamination` were left at defaults. No grid search or threshold optimization was performed.

**High false positives:** Precision of 0.22 means most samples flagged as attacks are actually normal operation — the model is noisy in both directions.

---

### Failure Modes

**Temporal blindness**

Isolation Forest scores each timestamp as an independent sample. It has no concept of sequence, drift, or trajectory. In ICS environments, attacks rarely manifest as sudden point anomalies — they appear as *gradual, sustained deviations* in tank levels, pressures, and pump flows over time. A model blind to context across time will structurally miss these.

**Global vs. local anomaly detection**

Isolation Forest identifies statistical outliers relative to the global data distribution. In water distribution systems, many attacks are *stealthy* — adversaries deliberately keep measurements within plausible operating ranges while slowly driving the system toward a failure state (e.g. tank overflow, pressure manipulation). These are local anomalies within a normal-looking neighborhood. Isolation Forest is not designed to catch them.

---

### Key Takeaway

In a cyber-physical system, an F1 of 0.25 on the attack class is not an acceptable baseline — it means the vast majority of attacks pass through undetected. The failure here is structural, not just a tuning problem. Adding temporal context is the necessary next step before any further tuning makes sense.

---

### Next Steps

- [ ] Sliding window feature engineering — encode temporal drift across a fixed lookback window
- [ ] Use Isolation Forest anomaly scores as features for a supervised stacker (XGBoost)
- [ ] Tune contamination against the actual attack ratio in Training Set 2 (~11.8%)
- [ ] Target: F1 > 0.60 on the attack class

---
