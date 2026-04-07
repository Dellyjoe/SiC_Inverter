# DC Input Protection Circuit – Test Notes

## 1. Test Setup

- Bench Supply: Riden RD6018W  

- Connections

&#x20; - RD6018W `+` → `+VIN`

&#x20; - RD6018W `−` → `−VIN` (GND)

### Circuit Under Test

&#x20; - F1 fuse

&#x20; - D6 (P6KE500A TVS)

&#x20; - Q8 (STB10NK60Z) N‑MOSFET

&#x20; - Gate network: R44, R45, R46, R47, C66, D7 (zener)

&#x20; - Output node: `+VIN\_Protected`

---

## 2. Forward‑Polarity, No‑Load Tests

Needs to be completed

### Conditions
- No load connected to `+VIN\_Protected`.

### VGS Measurements

| VIN | VGS |

|-----|------|

| 12 V | \~1.05 V |

| 48 V | \~4.34 V |

| 60 V | \~5.42 V |



### Interpretation

- Gate voltage scales at \~9% of VIN (below zener clamp region).

- MOSFET is weakly enhanced at low VIN and more enhanced at higher VIN.



---



## 3. Forward‑Polarity, No‑Load VDS (Voltage Drop Across Q8)



### Definition

`VDS = VIN − VIN\_Protected`



### VDS Measurements

| VIN | VDS        |
|-----|------------|
| 12 V | ~0.225 V  |
| 48 V | ~0.001 V  |
| 60 V | ~0 V      |




### Interpretation

- At higher VIN, Q8 behaves as a low‑loss pass element.

- Small drop at 12 V is expected and acceptable with no load.



## 4. Reverse‑Polarity Test



### Setup

RD6018W leads intentionally reversed:

- RD6018W `+` → `−VIN`

- RD6018W `−` → `+VIN`



### Observations

- No output voltage on `+VIN\_Protected`.

- No abnormal current draw from RD6018W.



### Conclusion

Reverse‑polarity protection works correctly.  

Q8 blocks reverse conduction and TVS does not falsely trigger.


## 5. Gate Clamp Behavior (Conceptual)



- Gate network is a resistor divider + zener clamp (D7).

- Below zener knee 

&#x20; Gate ≈ 9% of VIN (matches measured VGS at 12–60 V).

- \*\*Above zener knee:\*\*  

&#x20; VGS is clamped near zener voltage (\~15 V).  

&#x20; Excess voltage is dissipated through the divider + zener.



### Implication  

Gate oxide is protected from overvoltage.  

Divider + zener must be power‑rated appropriately if VIN increases.


## 6. Voltage Range and Safety Notes



- Circuit has been tested and validated up to \*\*60 V\*\* using RD6018W.

- Component choices and PCB layout indicate a \*\*low‑voltage DC input stage\*\*.

- Even though Q8 is a 600 V MOSFET and D6 is a 500 V TVS:

 - PCB creepage/clearance is not rated for 400 V.

 - Fuse breaking capacity is not rated for high‑energy HV faults.

- Gate divider would force significant current into the zener at high VIN.



### Conclusion

This design should \*\*not\*\* be used above \~60–80 V without redesign.


## 7. Next Planned Tests (With Load)

Once power resistors are available

1\. Add loads to `+VIN\_Protected`:

- 100 Ω, 47 Ω, 22 Ω (50–100 W resistors)

2\. For each VIN (12 V, 24 V, 48 V, 60 V):

  - Record \*\*VIN\*\*

  - Record \*\*load value\*\*

  - Record \*\*current (I)\*\* from RD6018W

  - Measure \*\*VDS (VIN − VIN\_Protected)\*\*

  - Check Q8 temperature (touch/IR/thermocouple)



# Goal

Verify Q8 dissipation under load and confirm the input stage is ready for inverter bring‑up.





