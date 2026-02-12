Antenna beamwidth
Angular separation between the points where the power drops to half of its maximum value (-3 dB)
- defines directivity and coverage area of the antenna


![[Pasted image 20260210161830.png]]


## Friis propagation model

$$P_{R}=G_{R} G_{T}(\frac{c}{4\pi f_{c}d})^\alpha P_{t}$$
- $f_{c}$: center frequency in Hz
- $d$: distance between transmitter and receiver
- $\alpha$: pass loss component (2 for line-of-sight environment)
- $G$: antenna gain

## Free Space Path Loss (FSPL)
How much power is lost purely because the signal spreads out in free space.
$$FSPL=(\frac{4\pi d}{\lambda})^2$$
- Higher frequency → smaller wavelength → larger loss

**Link Budget**
$$P_{r}(dBm)=P_{t}(dBm)+G_{t}​(dBi)+G_{r}(dBi)−FSPL(dB)$$
We can also derive from Friis:
$$FSPL(dB)=20\log_{10}(d)+20\log_{10}(f)+20\log_{10}(\frac{4\pi}{c})$$

