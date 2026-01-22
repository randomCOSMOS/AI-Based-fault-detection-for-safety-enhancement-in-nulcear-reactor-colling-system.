Create a simplified **cooling-loop / coolant tank system** using **first principles physics**, generate realistic sensor time-series, and then treat one sensor as “faulty”.

This gives:  
- “true system values” (ground truth)  
- sensor readings (noisy/faulty)  
- dataset with full control  
- very defendable in viva (“physics-grounded”)

# The simplest realistic model 
A cooling loop can be represented as:
### System blocks
1. **Tank / Pipe section** (has temperature state)
2. **Pump** (affects flow rate)
3. **Valve** (affects flow resistance)
4. **Heat input** (reactor core / heater)
5. **Heat loss** (to environment / heat exchanger)

---
# What variables to simulate (sensors)
You should simulate at least 3 sensors (minimum for virtual sensor correlation):
### Sensors (time-series)
- **T(t)** = coolant temperature (°C)
- **F(t)** = coolant flow rate (kg/s or L/s)
- **P(t)** = pressure (bar)

Optional:

- pump speed **N(t)**
- valve opening **V(t)**

---

# Core Physics Equations (very defendable)

You don’t need full CFD. Just conservation laws:
## 1) Temperature model (Energy balance)

This is the heart of the system.
Let:

- m = coolant mass in tank/loop
- Cp = specific heat capacity
- Qin = heat added (heater/reactor)
- Tin = inlet temperature
- ṁ = mass flow rate
- k_loss = thermal loss coefficient
- Tamb = ambient

**ODE:**  
$$ \frac{dT}{dt} = \frac{\dot{m}}{m}(T_{in}-T) + \frac{Q_{in}}{m C_p} - \frac{k_{loss}}{m C_p}(T - T_{amb}) $$

### Meaning:

- flow brings Tin and changes T
- heater adds energy
- losses cool it down

✅ This alone creates very “plant-like” temperature curves.

---

## 2) Flow model (Pump + Valve dynamics)

You can do flow in 2 ways:

### A) Simple control-based flow (easiest)

Assume pump speed sets flow:

$$
\dot{m}(t) = k_p N(t) \cdot V(t)  
$$

- N(t) = pump speed input
- V(t) = valve opening (0–1)


### B) Physics-based flow using pressure drop (more researchy)

Use:  
$$
\Delta P = R(V) \dot{m}^2  
\Rightarrow \dot{m} = \sqrt{\frac{\Delta P}{R(V)}}  
$$

- valve resistance increases when valve closes:  
$$
    R(V) = R_0 + \frac{R_1}{V^2+\epsilon}  
$$

This makes flow non-linear looks realistic.


## 3) Pressure model (optional but good)

Simplest:  
$$
P(t) = P_0 + k_{pf} \dot{m}(t) - k_{leak}(t)  
$$

Or dynamic:  
$$
\frac{dP}{dt} = a\dot{m}(t) - bP  
$$


# How you generate a dataset

## Step-by-step pipeline

### Step 1: Define inputs (operating changes)

Simulate realistic control patterns:

- pump speed steps up/down
- valve open/close
- heater power changes
- disturbance events

### Step 2: Solve ODEs numerically

Use:

- Euler method (easy)
- or `scipy.integrate.odeint/solve_ivp` (better)

### Step 3: Create sensor readings from true states

Sensor output = true + noise + delay:  
$$
y_{measured}(t) = y_{true}(t) + \eta(t)  
$$


# Fault injection (must-have for your dataset)

You will inject faults into **only the sensor measurement layer**, not the real physics state.

### Fault types and formulas:

## 1) Bias fault

$$
y_f(t) = y(t) + b  
$$

## 2) Drift fault

$$
y_f(t) = y(t) + \alpha (t - t_0)  
$$

## 3) Stuck-at fault

$$
y_f(t) = constant  
$$

## 4) Increased noise

$$
y_f(t) = y(t) + \mathcal{N}(0, \sigma^2_{high})  
$$

## 5) Dropout (missing sensor)

- replace with NaN
- or hold-last-value

# Why this is a VERY strong research dataset

### You can claim:

- “Physics-grounded digital twin style simulation”
- “Controlled injection of sensor faults”
- “Ground truth available for evaluation”
- “Multiple operating modes simulated”

This matches how many academic FDD papers validate when real nuclear plant data is restricted.

---

# Deliverables:
For faculty:
1. **System block diagram** (tank + pump + valve + heater)
2. Plots:
    - T_true vs T_sensor_faulty
    - residual trend during fault
    - fault detected timestamp
3. Dataset table sample:
    - time, T, P, F, pump_speed, valve_open, fault_label