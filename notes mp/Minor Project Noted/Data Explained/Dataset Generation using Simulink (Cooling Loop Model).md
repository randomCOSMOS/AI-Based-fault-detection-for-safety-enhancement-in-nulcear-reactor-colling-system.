## What it is

Since real Nuclear Power Plant (NPP) sensor datasets are restricted, we generate a realistic multivariate sensor dataset using a **MATLAB/Simulink-based cooling loop model** (digital-twin style). The model simulates coolant system dynamics and produces time-series sensor readings.
## Why we are using Simulink (relevance to nuclear topic)

Simulink allows us to represent **cooling instrumentation behavior** in a controlled way:

- coolant temperature, pressure, flow rate
- pump and valve operations
- disturbances and operating condition changes

This directly maps to nuclear reactor cooling sensor networks, where safety relies on continuous instrumentation monitoring.

## What we will simulate (signals / sensors)
### Process variables (true system states)
- **T(t): coolant temperature**
- **F(t): coolant flow rate**
- **P(t): coolant pressure**

### Control / actuator inputs

- **Pump speed N(t)**
- **Valve opening V(t)**
- Heater power / heat input **Q(t)** (represents core heating / load)

## How we will generate the dataset (technical steps)

### 1) Build simplified coolant loop model

Simulink blocks representing:

- thermal tank / heat transfer
- pump dynamics
- valve resistance
- sensor measurement blocks
- noise + delay blocks for realism

### 2) Run multiple operating scenarios

We generate diverse time-series by varying:

- pump speed profiles (step/ramp changes)
- valve position changes
- heat input changes (load transients)
- external disturbances (heat loss increase, pump degradation)
### 3) Fault injection (sensor fault dataset creation)

To create labelled faults, we inject faults in the **measurement layer** only:

Fault types:

- **Bias fault:** y_fault = y + k
- **Drift fault:** y_fault = y + αt
- **Stuck-at fault:** y_fault = constant
- **Noise fault:** y_fault = y + N(0, σ_high²)
- **Dropout fault:** missing / hold-last-value

This creates ground-truth labelled data:

- clean “true” value
- faulty sensor reading
- fault label + fault start time

## How this dataset supports our AI pipeline

We use the Simulink-generated dataset to:

### 1) Train Virtual Sensors (soft sensors)

**Models:** GRU/LSTM / XGBoost regression  
Goal: predict sensor value ŷ(t) using correlated signals.

### 2) Residual-based fault detection

**Residual:** r(t) = y(t) − ŷ(t)  
Fault alarm if residual shows abnormal trend/spike.

### 3) Fault diagnosis

**Models:** XGBoost / RandomForest / SVM multi-class  
Output: fault type (drift/bias/stuck/noise/dropout).
## Why this is research-grade

- Physics-grounded simulation (digital twin style)
- Controlled fault injection → labeled fault data
- Ability to test rare faults and extreme conditions
- Ground truth available for validation

---

## Results/graphs we will show

- T_true vs T_faulty vs T_virtual (model predicted)
- Residual plot highlighting fault onset
- Fault classification confusion matrix
- Metrics: RMSE, F1-score, FAR, detection delay