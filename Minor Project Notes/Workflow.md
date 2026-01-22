## **Phase 0 — Problem Definition (Nuclear Safety Context)**

1. Select coolant-loop instrumentation variables: **T(t), P(t), Flow(t), Pump speed, Valve position**
2. Define goal: **detect & diagnose sensor faults early** to prevent unsafe control actions.

**Gap solved (paper):** Nuclear systems require reliable early fault detection in complex coupled instrumentation.

---

## **Phase 1 — Multi-source Data Generation (Digital Twin + Benchmark Strategy)**

### **1A) [[ODE Simulation|Physics Digital Twin Dataset (ODE simulation)]]**

- Generate physics-consistent sensor time-series (energy/mass balance).
- Add realistic disturbances & measurement noise.

### **1B) [[Dataset Generation using Simulink (Cooling Loop Model)|High-fidelity Digital Twin Dataset (Simulink + fault injection)]]**

- Simulink cooling-loop generates **true** sensor values.
- Inject sensor faults: drift/bias/stuck/noise/dropout.
- Preserve **ground truth vs measured sensor**.
### **1C) [[Benchmark Dataset - Tennessee Eastman Process (TEP)|Benchmark Dataset (TEP)**]]

- Use TEP as standard FDD benchmark for generalization validation.

**Gap solved (paper): dataset scarcity / limited labeled nuclear fault data**  
→ simulation + fault injection gives controllable labelled faults; benchmark ensures research comparability.

## **Phase 2 — Dataset Construction & Standardization**

- Convert all sources to **common dataset schema**:
    - `time, sensors, actuators, fault_label, fault_start_time`

- Train/val/test splits in multiple operating conditions.

**Main recorded outputs here:**  
number of scenarios, fault cases per class, normal/fault windows.

---

## **Phase 3 — Preprocessing & Time-series Modelling Setup**
- Cleaning, scaling, missing handling
- **Windowing** into sequences (sliding windows)
- Create engineered features:
    - residual stats: mean, std, slope
    - temporal features: lag values, moving averages

**Gap solved (paper): generalization across conditions**  
→ multi-scenario simulation + standardization prepares robust training distribution.

## **Phase 4 — Virtual Sensor Modelling (Core AI Model)**

Train **virtual sensor (soft sensor)** to estimate critical measurement:

- Models: **GRU/LSTM / XGBoost regression**
- Input: correlated signals (T,P,F, actuator states)
- Output: predicted sensor `ŷ(t)`

**Primary output recorded:**  
Virtual sensor accuracy: **RMSE, MAE, R²**

**Why it matters:** Virtual sensors provide redundancy for safety when sensors degrade.

## **Phase 5 — Residual Generation (Fault Signature)**

Compute residual:

- `r(t) = y(t) − ŷ(t)`  
    (or KF innovation if applied)

Record residual features:

- magnitude, mean-shift, variance, slope, duration

**Primary output recorded:**  
Residual distribution under normal vs fault

## **Phase 6 — Fault Detection (Anomaly Detection)**

Detect fault onset using:

- **Unsupervised:** LSTM-Autoencoder / IsolationForest / One-Class SVM
- **Statistical baseline:** EWMA / CUSUM on residual

**Primary output recorded (“research metrics step”)**  
This is the **main recording** you were asking for:

##### Detection metrics (core research metrics):
- **Precision / Recall / F1-score**
- **False Alarm Rate (FAR)**
- **Detection Delay (seconds / timesteps)**

These metrics are the **base of inference**, since they prove:

- how early you detect faults
- how reliably you avoid false alarms

**Gap solved (paper): need for reliable early fault detection**  
→ detection delay + FAR directly address this.

## **Phase 7 — Fault Diagnosis (Fault Type Identification)**

If fault detected, classify fault type:

- drift / bias / stuck-at / noise / dropout
- Models: **XGBoost / RF / SVM / MLP**

**Primary output recorded:**  
Diagnosis metrics:
- confusion matrix
- per-class F1 scores

**Gap solved (paper): FDD should go beyond detection to diagnosis**
## **Phase 8 — Nuclear Safety Layer (Novelty)**

### **8A) Sensor Health Index (QoS score)**

- Compute reliability score 0–1 using residual behavior.

### **8B) Severity & Impact estimation**

- Severity prediction: low / medium / high
- Based on residual magnitude + slope + persistence.

### **8C) Fail-safe substitution**

- If sensor unreliable → switch to virtual sensor output + confidence score.

**Primary output recorded:**  
- Sensor health trend & severity correctness  
- Safe/unsafe decision threshold” behavior

**Gap solved (paper): AI should estimate severity/impact, not just detect fault**

---

## **Phase 9 — Explainability (XAI)**

Explain each alarm:

- SHAP (SHapley Additive exPlanations) feature contribution
- residual reasoning: “which sensor mismatch caused alarm”

**Primary output recorded:**  
- top contributing features per fault decision  
- explanation plots (bar/force plots)

**Gap solved (paper): need for explainable models (XAI) for safety trust & deployment**

## **Phase 10 — Cross-validation across Data Sources (Research proof)**

Validate same pipeline on:

- ODE dataset
- Simulink dataset
- TEP benchmark dataset

**Primary output recorded:**  
- Robustness / generalization metrics.
- performance stability across datasets
- drop in accuracy across domains

**Gap solved (paper): generalization and reliability across systems**
# General Outputs (what your project finally produces)

These are your deliverables:

1. **Virtual sensor predictions** (ŷ) + error bounds
2. **Fault alarms** + anomaly score
3. **Fault type** (drift/bias/stuck/noise/dropout)
4. **Sensor health score (QoS)**
5. **Severity & impact estimate**
6. **Explainable reason (XAI)**
7. **Research metrics dashboard** proving reliability and safety usefulness