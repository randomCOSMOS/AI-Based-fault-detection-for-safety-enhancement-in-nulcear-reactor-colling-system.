## 1) Real raw nuclear plant data

### What it is: 
Actual NPP instrumentation data from cooling systems (PWR/BWR/PHWR): temperature, pressure, flow, pump speed, valve states.
### How to get
- Public nuclear datasets (rare)
- institutional/partner access
- published supplementary datasets

>Most authentic  
>Almost impossible to obtain at student level (restricted + security)

### **2) [[ODE Simulation|Physics-based ODE Simulation in Python/Matlab (No Simulink Dependency)]]**
**What:** Write simplified thermal-hydraulic equations (mass/energy balance) to generate sensor streams.  
**Technical:** Solve ODEs + add noise/disturbances → generate realistic temp/flow/pressure time-series.

### **3) [[Benchmark Dataset - Tennessee Eastman Process (TEP)|Benchmark Fault Detection Dataset (Fastest + Legit Research Standard)]]**
**What:** Use widely accepted multivariate FDD datasets.  
**Technical:** Tennessee Eastman Process (TEP) dataset / similar process datasets with labeled faults.

### **4) [[Dataset Generation using Simulink (Cooling Loop Model)|Simulink + Fault Injection (Best Research-grade Setup)]]**
**What:** Generate clean “true” sensor values in Simulink, then inject sensor faults in measurement layer.  
**Technical:** Inject drift/bias/stuck-at/noise/dropout → keep ground truth for evaluation.