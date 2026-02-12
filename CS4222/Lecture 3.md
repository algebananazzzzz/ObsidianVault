## Effective Isotropic Radiated Power (EIRP)
Total radiated power if the antenna were an ideal isotropic (omni-directional) radiator
- Combines transmitter power and antenna gain into a single metric
- A high-gain antenna with low transmitter power can produce the same EIRP as a low-gain antenna with high transmitter power
$$
EIRP=P_{\text{tx}}+G_{\text{antenna/amp}}-L_{\text{cable/medium}}
$$

## Rules for Combining dB, dBm, dBi
|Unit|What It Represents|Type|
|---|---|---|
|**dB**|Power ratio|Relative|
|**dBm**|Absolute power (referenced to 1 mW)|Absolute|
|**dBi**|Antenna gain relative to isotropic|Ratio
**Allowed**
1. **dBm + dB:** Apply gain or loss to an absolute power.
   $P_{out}=$Transmitter power (dBm) - Cable loss (dB)
2. **dBm + dBi:** Apply antenna gain to transmit power.
   **EIRP** = Transmit power (dBm) + Antenna gain (dBi)
- dBi + dBi (Combining Antenna Gains):  $G_{tx}+G_{rx}$
- dBm + dBm:
  **Link margin** = Received power - Receiver sensitivity
  
**Not Allowed**
- dBm + dBm (both are log scale)
- dBi + dBm (adding ratio to absolute value)
