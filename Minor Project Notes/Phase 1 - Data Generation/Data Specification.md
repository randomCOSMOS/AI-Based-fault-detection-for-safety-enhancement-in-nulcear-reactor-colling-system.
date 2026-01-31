# Common Variables:
| Variable | Meaning                 | Unit       |
| -------- | ----------------------- | ---------- |
| Tin      | Core inlet temperature  | °C         |
| Tout     | Core outlet temperature | °C         |
| P        | Primary loop pressure   | bar        |
| F        | Coolant mass flow rate  | kg/s       |
| RPM      | Pump speed              | rpm        |
| V        | Valve position          | 0–1 (or %) |
### [[Dataset Generation using Simulink (Cooling Loop Model)|Simulink]] Unique:
| Variable | Meaning                                    | Unit           |
| -------- | ------------------------------------------ | -------------- |
| L        | Pressurizer level (or system volume proxy) | % or arbitrary |
### [[ODE Simulation|ODE]] Unique:
| Variable | Meaning                   | Unit  |
| -------- | ------------------------- | ----- |
| h        | Heat transfer coefficient | W/m²K |
# Units:
- Temperature → °C
- Pressure → bar
- Flow → kg/s
- RPM → rpm
- Valve → 0–1
- Level → %
- h → W/m²K

# [[Data Specs Info|Sampling Rate:]]
### [[Dataset Generation using Simulink (Cooling Loop Model)|Simulink]]:
- Fixed‑step solver
- Step size: 0.1 seconds
### [[ODE Simulation|ODE]]:
- Evaluating Solutions at: t = np.arange(0, 200, 0.1)

# [[Data Specs Info|Fault Profiles: ]]
## Noise:
μ = 0
σ = 0.05
## Bias:
- Constant offset: +0.5
- Starts at: t = 60s
## Drift fault:
- Linear Drift: 0.02 × (t − 60)
- Starts at: t = 60s
## Stuck-at fault:
Freeze value after: 120s
## Dropout (optional):
Set signal to zero for 140s -> 150s

# Output Structure:
t,Tin,Tout,P,F,RPM,V,L,fault
0.0,...
0.1,...


