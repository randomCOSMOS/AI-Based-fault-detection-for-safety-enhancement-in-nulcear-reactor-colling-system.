# Base Specs:
- Type: Fixed Step
- Solver: ide4 (Runga Kutta)
	- Continuous dynamics (Transfer Fcn, Integrator)
	- Smooth signals (flow, temperature, pressure)
	- Fault injection (drift, bias, noise)
	- ML needs **uniformly sampled data**
- Step Size: 0.1
- Stop Time: 200
# Starting Values:
## Pump Speed (RPM):
- Initial = 1500
- Final = 2500
- Step = 5sec
## Coolant Flow (F):
- Formula
```c
F = 0.01 × ( 1 / (8s + 1) ) × RPM
```
- RPM goes through a **first-order lag** (8s time constant).
## Valve Position (V):
- V  = 0.8
- Valve is 80% open.
## Core Inlet Temperature (Tin):
- Tin = 280 ℃
## Core Outlet Temperature (Tout):
- Formula
```c
Tout = ( 1 / (5s + 1) ) × [ Tin + 0.05 × F ]
```
- Heat rises proportional to Flow.
- 5s Thermal Delay.
## Pressure (P):
- Formula
```c
P = ( 1 / (6s + 1) ) × [ 2 × F − V ]
```
- Flow ↑  = Pressure ↑
- Valve Opening ↑ = Pressure ↓
## Pressurizer Level (L):
- Formula
```c
L = 0.01 × ∫ (P − 150) dt
```
- Taking Nominal Pressure = 150
- Level Rises if P > 150, and vice versa.

# Faults:

| **Time**    | **Fault**        |
| ------- | ------------ |
| 0-60    | Normal       |
| 60+     | Bias + Drift |
| 120+    | Stuck At     |
| 140-150 | Dropout      |
## Bias
- +0.05 at 60 secs
## Drift
- +0.02 slope
## Stuck Values
- RPM: 2500
- F: 5
- Tout: 300
- P: 150
## Not chosen but possible Stuck Values
- V: 0.8  
- L: 50
- Tin: 280
> Generally these variable stay clean in the real settings.
# Output:
## CSV

|              Tin |              Tout |                  P |                  F |              RPM |                 V |                  L | fault_label |
| ---------------: | ----------------: | -----------------: | -----------------: | ---------------: | ----------------: | -----------------: | ----------: |
| 280.252096706584 | 0.252096706584515 |  0.252096706584515 |  0.252096706584515 | 1500.25209670658 |  1.05209670658452 |  0.252096706584515 |           0 |
|  281.68762428156 |  7.23208848890035 |    1.6774962299628 |   1.87395727411357 | 1501.68762428156 |  2.48762428155986 |   1.53761868584709 |           0 |
| 279.393705923347 |  10.3730299210459 | -0.620261594980096 | -0.235942757153663 | 1499.39370592335 | 0.193705923346516 | -0.906312236673858 |           0 |
| 280.908862174608 |  17.2156095811877 |  0.897163794698737 |   1.46094590868416 | 1500.90886217461 |  1.70886217460787 |  0.458830679950214 |           0 |
| 280.138731248736 |  21.6675908165314 |  0.135234552039898 |  0.870289881078414 | 1500.13873124874 |  0.93873124873579 | -0.461308330637276 |           0 |
| 279.725720226056 |  26.3734637886862 | -0.263814460127877 |  0.634524283672917 | 1499.72572022606 | 0.525720226056109 |  -1.02431634199154 |           0 |
## Labels:
- 0 => Normal
- 1 => Bias/Drift
- 3 => Stuck
- 4 => Dropout