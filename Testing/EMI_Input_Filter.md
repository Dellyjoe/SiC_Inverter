# EMI Input Filter – Testing Document

## 1. Overview

This document captures the test procedure, setup, and measured performance of the EMI Input Filter used between `+VIN_EMI` and `BUS+_Filtered`.  
The filter consists of:

- **L1 = 47 µH** (series)
- **C1 = 1 µF** (shunt to GND)
- **L2 = 2.2 mH** (series)(CMC)

This forms a two‑stage differential‑mode LC low‑pass filter intended to suppress inverter switching noise from propagating back toward the DC source.

---

## 2. Test Equipment

- **Function Generator**
  - Sine wave output
  - 1 Vpp amplitude
  - Output load set to **High‑Z / 1 MΩ**
- **Oscilloscope**
  - Two channels (CH1 = Vin, CH2 = Vout)
  - DC coupling
  - Matching probe attenuation (1×)
---

## 3. Test Setup

### Connections

- Function Gen `+` → `+VIN_EMI`
- Function Gen `−` → GND
- Scope CH1 → `+VIN_EMI`
- Scope CH2 → `BUS+_Filtered`
- Scope grounds → GND

### Notes
- Verify the function generator produces the correct amplitude by connecting it directly to the scope before testing the filter.
---

## 4. Test Procedure

1. **Baseline Verification**
   - Set function generator to **1 kHz, 1 Vpp**.
   - Confirm **Vin ≈ Vout** (flat passband behavior).

2. **Frequency Sweep**
   - Keep amplitude at **1 Vpp**.
   - Measure Vin and Vout at the following frequencies:
     - 0.01 kHz  
     - 0.1 kHz  
     - 1 kHz  
     - 10 kHz  
     - 50 kHz  
     - 100 kHz  
     - 200 kHz  
     - 500 kHz  
     - 1000 kHz (1 MHz)

3. **Record Data**
   - Capture Vin (Vpp), Vout (Vpp), and compute attenuation.

---

## 5. Measured Results

### Raw Data

| Freq (kHz) | Vin (Vpp) | Vout (Vpp) |
|-----------:|-----------|------------|
| 0.01       | 1.000 V   | 1.000 V    |
| 0.1        | 0.983 V   | 0.999 V    |
| 1          | 0.378 V   | 0.378 V    |
| 10         | 0.034 V   | 0.042 V    |
| 50         | 0.030 V   | 0.010 V    |
| 100        | 0.071 V   | 0.0057 V   |
| 200        | 0.148 V   | 0.0020 V   |
| 500        | 0.390 V   | 0.0015 V   |
| 1000       | 0.810 V   | 0.0012 V   |

### Calculated Attenuation

| Freq (kHz) | Vout/Vin | Attenuation (dB) |
|-----------:|----------|------------------|
| 0.01       | 1.00     | 0.0 dB           |
| 0.1        | 1.02     | +0.2 dB          |
| 1          | 1.00     | 0.0 dB           |
| 10         | 1.24     | +1.9 dB          |
| 50         | 0.33     | −9.6 dB          |
| 100        | 0.08     | −22.0 dB         |
| 200        | 0.0135   | −37.4 dB         |
| 500        | 0.00385  | −48.3 dB         |
| 1000       | 0.00148  | −56.6 dB         |

---

## 6. Interpretation

### Passband (0–1 kHz)
- Filter is flat and stable.
- No significant attenuation.
- Suitable for inverter low‑frequency bus ripple and control‑loop dynamics.

### Transition Region (10–50 kHz)
- Begins attenuating around 10–20 kHz.
- ~−10 dB by 50 kHz.

### Stopband (100 kHz–1 MHz)
- Strong attenuation:
  - ~−22 dB at 100 kHz  
  - ~−37 dB at 200 kHz  
  - ~−48 dB at 500 kHz  
  - ~−57 dB at 1 MHz  
---

## 7. Practical Impact on the System

If the inverter produces ~1 Vpp differential‑mode ripple on the DC bus:

- At **50 kHz** → ~0.33 Vpp reaches +VIN_EMI  
- At **100 kHz** → ~80 mVpp  
- At **200 kHz** → ~13 mVpp  
- At **500 kHz** → ~4 mVpp  
- At **1 MHz** → ~1.5 mVpp  

This significantly reduces conducted noise back into the power supply or battery.

---

## 8. Conclusion

The EMI Input Filter performs as designed:

- Flat response in the operating band (0–1 kHz)
- Strong attenuation in the inverter switching band (50–200 kHz)
- Very deep attenuation at high harmonics (500 kHz–1 MHz)

This filter will effectively prevent inverter switching noise from contaminating the upstream DC source.

