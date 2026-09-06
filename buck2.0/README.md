# 50V-in Buck Supply — Design Notes

Used **SIM PE by Renesas** to verify the design at both **3.3V and 20V output**, basically both extremes of the output range. checked the main cases in sim before locking the values.
<img width="1912" height="1155" alt="image" src="https://github.com/user-attachments/assets/cb2262c4-b6fb-4c33-86b3-60573f100298" />
<img width="1891" height="1151" alt="image" src="https://github.com/user-attachments/assets/9ac9a627-137a-4da0-9181-3ef3847c0101" />
<img width="1361" height="932" alt="image" src="https://github.com/user-attachments/assets/dc1f0fdf-7d97-4856-b0c2-73643b081311" />
(excuse the messy wiring)

This is a buck supply. 50V in, 3.3V–20V out at 6A. separate 5V/3A rail for logic + current sense IC. gonna go thru the main parts + math.

## why a controller and not a regulator

At 50V in and 6A out, a regulator with the FET built in would run way too hot. external FETs make more sense here. so controller it is.

## why ISL8117 specifically

Looked at LM5116, LM5117, LTC3891 etc and went with ISL8117 cuz:

* input range 4.5V–60V, so 50V is fine
* output range 0.6V–54V, covers 3.3–20V
* fully synchronous
* internal loop compensation, so no COMP network to tune
* only 16 pins
* current mode control + input feedforward
* easy enough to source from mouser/digikey

Went with **HTSSOP** cuz the high side and low side gate pins are on the same side, so PCB routing is easier. also easier to inspect than QFN.

## MOSFETs — Q1 / Q2

Using the **same MOSFET model, CSD19531Q5A**, for both Q1 high side and Q2 low side.

why:

* enough voltage margin above 50V
* work with the 5V gate drive
* low rDS(on)
* gate charge is low enough for 500kHz

even at max ripple, the MOSFET heat is only around **80°C**, so its fine.

## RT — switching frequency

Using **500kHz**.

IC eq:

```text
RT [kΩ] = 39.2/fSW[MHz] − 1.96
```

For 500kHz:

```text
RT = 39.2/0.5 − 1.96
   = 76.44kΩ
```

At the lowest output, 3.3V from 50V:

```text
D = 3.3/50 = 6.6%
```

Using the 40ns minimum on-time:

```text
f_max = D_min / tON_MIN
      = (3.3/50) / 40ns
      ≈ 1.65MHz
```

500kHz is well below that, so fine.

## Soft start cap — 6.04nF

```text
t_SS = CSS × 0.6V / 2µA
```

With 6.04nF:

```text
t_SS ≈ 1.8ms
```

above the IC minimum, so the external cap sets the ramp.

## Output voltage set point — DAC controlled FB divider

FB regulates to **0.6V**.

Topology: R1 from VOUT to FB, R2 from FB to SGND, R3 from DAC output to FB.

FB equation:

```text
(Vout − 0.6)/R1 − 0.6/R2 + (Vdac − 0.6)/R3 = 0
```

Needed limits:

```text
Vdac = 0V   → Vout = 20V
Vdac = 3.3V → Vout = 3.3V
```

Set R2 using 100µA divider current:

```text
R2 = 0.6V / 100µA
   = 6.0kΩ
   → 6.04kΩ
```

Solved values:

```text
R1 = 165kΩ
R3 = 32.4kΩ
```

With standard values:

```text
Vdac = 0V   → Vout ≈ 20.05V
Vdac = 3.3V → Vout ≈ 3.24V
```

## EN divider — R7 / R8

EN threshold = 1.6V typ.

```text
Vin(turn-on) = VEN × (R7+R8)/R7
             = 1.6 × (10k+243k)/10k
             = 40.5V
```

Using:

```text
R7 = 10.0kΩ
R8 = 243kΩ
```

so the converter starts around 40.5V input.

## Current sense / OCP — RCS / ROCSET

This IC senses current using the **low side FET rDS(on)**, no external sense resistor.

```text
RCS = (Imax × rDS(on)) / 30µA

ROCSET = (rDS(on) × IOC) / (0.7 + 3.5×RCS)
```

Using the current values:

```text
RCS = 3.3069kΩ
ROCSET = 8.45kΩ
```

## CBOOT

```text
CBOOT = QGATE / ΔVBOOT
```

Sized from the high side FET gate charge + allowed gate drive droop.

## Inductor — L1 = 5.21µH

```text
ΔIL = (Vin − Vout) × Vout / (fSW × L × Vin)
```

Ripple was estimated at around **70% of full load current**, since the datasheet mentioned 70% as the max recommended ripple.

Checked both **3.3V and 20V** cases since this is variable output.

## Output cap

Output cap was selected by trial and error in **SIM PE** using the load transient.

Tested the **0→6A load step** at both **3.3V and 20V** and adjusted the capacitance until the transient response was acceptable.

Used a combination of **ceramic + electrolytic capacitors**. ceramic helps with the faster part of the transient, electrolytic helps with the larger bulk capacitance.

## EXTBIAS — external 5V rail

The IC can generate its own 5V bias from Vin, but with 50V input that wastes power.

Already have a separate 5V/3A rail, so using that for **EXTBIAS**. once EXTBIAS is above about 4.7V, it feeds VCC5V and bypasses the internal LDO.

## separate 5V/3A rail — BD9G341EFJ

Used an **asynchronous buck** for the 5V/3A rail.

Main reason was that synchronous bucks operating from around **50V input** generally have a higher pin count, while this rail doesnt really need that extra complexity.

The **BD9G341EFJ datasheet already gives the component values for 50V input → 5V/3A output**, so there wasnt much ambiguity in setting the output.

### EN/UVLO divider — R13 / R14

This IC has an internal current source on EN. threshold is 2.6V.

wanted turn-on at 45V with around 3V hysteresis.

```text
R13 = 3V / 10µA
    = 300kΩ
    → 301kΩ

R14 = 2.6 × 301k / (45 − 2.6)
    = 18.458kΩ
    → 18.4kΩ
```

Using the standard values:

```text
turn-on ≈ 45.13V
turn-off ≈ 42.12V
```

## Series resistor from +BATT to VIN — R9

Added a small series resistor from +BATT to VIN as recommended by the datasheet. mainly to stop excessive current / possible backfeed into VIN from the 5V side.


