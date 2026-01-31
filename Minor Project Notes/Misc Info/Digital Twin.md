A **Digital Twin** is a **virtual replica** of a real physical system that continuously represents the system’s behavior using:

- **physics-based models** (equations/simulation)
- **real sensor data**
- sometimes an **AI/ML correction layer**
# Usage:
## Predict “expected normal behavior”

Twin outputs what values the sensors _should_ show.  
That becomes your **reference baseline**.
## Fault detection
Compare:
- **real sensor reading** 
- **twin predicted value**
The mismatch = **residual**  
Residual spike = fault/anomaly

## Fault diagnosis & what-if analysis
If a fault happens, twin can simulate:
- “what if pump efficiency drops?”
- “what if valve is stuck?”  
and compare patterns.

## Forecasting / predictive maintenance
Twin helps predict how system will behave next → **early warning**

# In our case:
[[ODE Simulation|ODE model]] and [[Dataset Generation using Simulink (Cooling Loop Model)|Simulink]]  are Digital Twins!