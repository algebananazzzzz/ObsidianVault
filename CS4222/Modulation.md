### Amplitude Modulation (AM)
**Modulation Index**
Extent to which the carrier signal is modulated with the message signal. Determines the strength and bandwidth of the modulated signal.
$$
s(t)=[A_{c}+A_{m}\cdot m(t)]\cdot \cos(2\pi f_{c}t)
$$
- $A_{c}$: Peak amplitude of carrier signal
- $A_{m}$: Peak amplitude of modulation (baseband) signal
- $m(t)$: Message signal
- $f_{c}$: Carrier frequency

$\mu < 1$: Not fully modulated; no distortion but signal may be weak
$\mu < 1$: Fully modulated
$\mu > 1$: Overmodulation; signal is distorted leading to clipping

### Frequency Modulation (FM)
- More resilient to interference, spectrum efficiency
$$
s(t)=A_{c}\cos( 2\pi f_{c}t+2\pi k_{f} \int m(t)dt)
$$
- $A_{c}$: Peak amplitude of carrier signal
- $f_{c}$: Carrier frequency
- $k_{f}$: Frequency sensitivity (frequency change per unit amplitude)
- $m(t)$: Message signal
### Phase Modulation (PM)
- Much better signal-to-noise ratio, spectrum efficiency
$$
s(t)=A\cos( 2\pi f_{c}t+2\pi k_{p}m(t))
$$
- $A$: Peak amplitude of carrier signal
- $f_{c}$: Carrier frequency
- $k_{p}$: Phase sensitivity (radians per unit amplitude)
- $m(t)$: Modulating signal
**Binary Phase Shift Keying**

| Bit 0 → Phase 0 degrees<br>Bit 1 → Phase 180 degrees | ![[Pasted image 20260212175957.png]] |
| ---------------------------------------------------- | ------------------------------------ |
**Quadrature Binary Phase Shift Keying**
Doubles the bit rate for same bandwidth
- Bit ‘00’ → Phase 45 degrees
- Bit ‘01’ → Phase 90 degrees
- Bit ‘11’ → Phase 135 degrees
- Bit ‘10’ → Phase 225 degrees

### Packet Delivery Ratio (PDR)
**Ratio of received packets to transmitted packets**
$$
PDR = \frac{\text{Received Packets}}{\text{Transmitted Packets}}
$$
### Bit Error Rate (BER)
**Measures the number of bit errors per unit time**
$$
BER = \frac{\text{Errored Bits}}{\text{Total Transmitted Bits}}
$$

### Signal-to-Noise Ratio (SNR)
**Higher SNR indicates stronger signal, link quality, fewer errors**
$$
SNR = 10\log_{10}(\frac{P_{\text{signal}}}{P_{\text{noise}}})
$$
- $> 25 dB$: Excellent (High-speed Wi-Fi, 4G LTE)
- $15-25 dB$: Good (Usable but may have errors)
- $< 10 dB$: Poor (Likely unreliable)
### Expected Transmission Count (ETX)
Predicts the number of transmissions required for successful delivery

## Shannon Capacity
$$
C = B \cdot \log_{2}(1+\frac{S}{N})
$$
- $C$: Channel capacity in bps
- $B$: Bandwidth of channel in Hz
- $S/R$: Signal-to-noise ratio