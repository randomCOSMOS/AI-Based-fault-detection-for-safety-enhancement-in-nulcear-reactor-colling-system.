# Sampling Rate:
How often you record data from your sensor.
**Importance:** AI models (LSTM / GRU / Autoencoder) expect **evenly spaced time‑series**.

>Sampling rate controls **time resolution**.
>Too slow → you miss faults  
>Too fast → unnecessary noise + big files

# Fault Profiles:
A **fault profile** is just a mathematical way to simulate: “How a sensor goes bad over time.”
## Noise:
Small random fluctuations are added continuously to the sensor signal.
**In our scenario:** Electrical interference near the pump or vibration in the coolant loop causes the **flow or pressure sensor** to fluctuate even though the plant is operating normally.
## Drift Fault:
Sensor slowly becomes inaccurate over time.
**In our scenario:** The **core outlet temperature sensor (Tout)** gradually degrades due to thermal ageing. The reactor is normal, but the sensor slowly reports higher temperature.
## Bias:
Sensor suddenly starts reading with a constant offset.
**In our scenario:** The **pressure transmitter or temperature probe loses calibration**, so it permanently reports a higher value than the true coolant condition.
## Stuck-At Fault:
Sensor freezes at one value and stops updating.
**In our scenario:** The **pump RPM feedback or valve position signal** freezes because of an ADC/channel failure or communication loss.  The pump may still be changing speed, but the control system keeps seeing the _same RPM value_.
## Dropout Fault:
Sensor disappears temporarily.
**In our scenario:** The **valve position or pressure signal briefly drops out** due to network or DAQ interruption during operation.
