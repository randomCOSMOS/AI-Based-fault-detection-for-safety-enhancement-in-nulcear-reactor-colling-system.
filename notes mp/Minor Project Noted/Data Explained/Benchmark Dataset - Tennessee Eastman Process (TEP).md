## What it is

The **Tennessee Eastman Process (TEP)** dataset is a standard benchmark used in research for **Fault Detection and Diagnosis (FDD)** in complex industrial control systems. It simulates a chemical process plant with multiple interacting units and control loops, generating realistic **multivariate time-series sensor data** under both normal and faulty conditions.
## Why we are using it (relevance to our nuclear cooling sensor project)

Even though it is not a nuclear dataset, it closely matches nuclear cooling instrumentation in terms of:

- tightly coupled sensor variables (temperature, pressure, flow-like signals)
- feedback control behavior
- multi-sensor fault propagation patterns
- availability of multiple labeled fault scenarios

So it is ideal to validate our **AI-based virtual sensor + fault detection pipeline** when direct NPP sensor datasets are restricted.

## What data it contains

TEP provides:

- **multivariate time-series data**
- around **50+ variables** (process measurements + manipulated variables)
- multiple fault types (commonly ~20+ standard fault classes depending on version)
- labeled sequences: **Normal** and **Faulty** operation windows
## How we’ll use TEP in our project (technical plan)

### 1) Virtual sensor modelling

- Choose one sensor variable as target **y(t)** (ex: temperature-like measurement)
- Train model to predict it using others:  
    **GRU/LSTM / XGBoost regression → ŷ(t)**

### 2) Residual-based fault detection

- Residual:  
    **r(t) = y(t) − ŷ(t)**
- Fault detection using:
    
    - **threshold/CUSUM/EWMA** on residual
    - or **Autoencoder anomaly score** / **Isolation Forest** on residual window features

### 3) Fault diagnosis (fault type classification)

- Using labeled faults:  
    **XGBoost / RandomForest / SVM multi-class classifier**
- Output: fault type (drift-like, bias-like, abnormal behavior class)
## Outputs we will show (for results)

- Actual vs Predicted sensor value plot (virtual sensor validation)
- Residual plot showing fault onset spike
- Confusion matrix (fault class classification)
- F1-score + False Alarm Rate + Detection Delay results