## Transmission bands
Each country has a **regulatory authority** 

**Wireless communication:** license-free Industrial, Scientific, and Medical (ISM) bands

## Antennas

- Radio waves: 3 Hz – 300 GHz
- Reciprocal (Tx = Rx performance)

| Omnidirectional                                           | Directional                                   |
| --------------------------------------------------------- | --------------------------------------------- |
| Radiates equally in all directions (isotropic)            | Focuses energy in one main direction (dipole) |
| Low gain (0–6 dBi)                                        | High gain (8–30+ dBi)                         |
| Short to medium range<br>More susceptible to interference | Long range<br>Less interference outside beam  |

**higher $f$** → **lower $\lambda$** (smaller antenna)
**lower $f$** → usually longer range

**Half-wave dipole:** $L=\frac{\lambda}{2}$
**Quarter-wave monopole:** $L=\frac{\lambda}{4}$

## Antenna gain

**Linear Gain**
Dimensionless quantity that is the ratio of the ability of an antenna to direct or receive radio waves in a particular direction compared to a reference. 

Higher the gain, more directional is the transmission and reception, and the narrower would be the beam

$$
G = \frac{\text{Power density in given direction}}{\text{Power density of reference}} = \frac{4\pi A_e}{\lambda^2} = \frac{4\pi f^2 A_e}{c^2}
$$
 - G = antenna gain
 - $A_{e}$ = effective area
 - f = carrier frequency
 - c = speed of light 3 x $10^8$ m/s

**Attenuation:** Means loss of energy
**Gain:** Means increase in energy

 Gain measured in:
- dBi (vs isotropic)
- dBd (vs half-wave dipole)
- $G_{dBi}=10\log_{10}G$
- $G_{\text{dBi}}​=G_{\text{dBd}}+2.15$

## Decibels (Power Ratio)

$$
dB=10\log_{10}\frac{P_{2}}{P_{1}}
$$

**Common dB values**

|dB Change|Power Ratio|
|---|---|
|+3 dB|×2|
|−3 dB|÷2|
|+10 dB|×10|
|+20 dB|×100|
|+30 dB|×1000|

### dBm (Absolute Power)

$$
P_{dBm}=10log_{10}​(P_{mW}​)
$$
**Key Reference Values**

| Absolute power | Power     |
| -------------- | --------- |
| $0 dBm$        | $1 mW$    |
| $30 dBm$       | $1 W$     |
| $-30 dBm$      | $1 \mu W$ |
