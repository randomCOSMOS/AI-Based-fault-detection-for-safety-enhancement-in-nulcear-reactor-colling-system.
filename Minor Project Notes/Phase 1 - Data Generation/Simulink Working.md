# Base Specs
- Type: Fixed Step
- Solver: ide4 (Runga Kutta)
	- Continuous dynamics (Transfer Fcn, Integrator)
	- Smooth signals (flow, temperature, pressure)
	- Fault injection (drift, bias, noise)
	- ML needs **uniformly sampled data**
- Step Size: 0.1
- Stop Time: 200

# Faults

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
## Labels:
- 0 => Normal
- 1 => Bias/Drift
- 3 => Stuck
- 4 => Dropout