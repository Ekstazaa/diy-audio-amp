## Input stage configurations

For the input stage, the most common solution is a differential pair. The main advantage of this configuration is the ability to apply negative feedback, which can be used to compensate for various non-idealities. For this chapter I will explain basic work of differential pair, parameters and some improvements.

## JFET vs BJT

In audio design books, the best choice for input stages for power amplifiers are.. BJT. The main reasons are their higher linearity, greater gain, and better temperature stability compared to FETs. The only real drawback of BJTs is the presence of base current, which reduces input impedance. However, in most cases, this trade-off is worth it due to the other significant advantages.

# Differential pair

![image](https://github.com/user-attachments/assets/726ff399-3be6-479d-9154-1d076e55de30)

A differential pair is a circuit built from two **identical** transistors with same load supplied by a constant current source. The core idea is that the current through each transistor depends on the voltage difference between the two inputs. If both inputs are equal, the current splits evenly — each side conducts I/2. When vin1 is higher than vin2, more current flows through the transistor on the right side what causes higher voltage on vout2 and vice versa. 

<img src="https://github.com/user-attachments/assets/5b9465f2-5fcf-4630-8122-23765ab88b20" width="60%" />

At start let's look at the chart above. It shows how the current through resistors R1 and R2 changes depending on the voltage difference between the inputs (note: negative values indicate that vin1 < vin2).
This graph has few conclusions:

1. The characteristic curve assumes perfectly matched transistors, which means the current splits evenly (I/2) when the voltage difference is exactly 0 V. In reality, transistors can have slight mismatches due to manufacturing tolerances, leading to input offset current or voltage, even when vin1 = vin2.
2. The linear operating region of the differential pair is quite narrow (it is assumed that narrow has 4<sub>*</sub>V<sub>T</sub> = 100 mV for normal configuration). This means the input stage is very sensitive, and even a small voltage difference between the inputs can lead to a significant imbalance in output current - in other words differencial pair has huge gain.
3. As we move further outside the linear range, one of the transistors starts to dominate entirely, conducting nearly the full bias current, while the other one effectively shuts off. At this point, the circuit loses its linear behavior and enters a region where the output no longer changes proportionally to the input difference. In practical terms, this limits show how large the differential input voltage can be **before distortion appears**.

In audio designs the input signal is kept within the linear region otherwise we can introduce distortions to our circuit.

## Gains of differental pair

Just like the differential pair has two inputs, it also has two outputs. Because of that, we can define three different types of voltage gain, depending on how we observe the outputs:
1. Gain measured at `vout1`:

**A<sub>v1</sub> = -R<sub>C</sub> / (2 · r<sub>eb'</sub>) = -1/2 · g<sub>m</sub> · R<sub>C</sub>**

2. Gain measured at `vout2`:

**A<sub>v2</sub> = R<sub>C</sub> / (2 · r<sub>eb'</sub>) = 1/2 · g<sub>m</sub> · R<sub>C</sub>**

3. Differential gain:

**A<sub>v_diff</sub> = R<sub>C</sub> / r<sub>eb'</sub> = g<sub>m</sub> · R<sub>C</sub>**

Where:
- `RC` is the collector resistance,
- `reb' = VT / IC` is the internal emitter resistance,
- `VT` is the thermal voltage (≈ 25 mV at room temperature).

## Input Resistance

As I mentioned earlier when discussing BJTs, one of the key issues is the input impedance, since a continuous base current flows into the transistor.

From the perspective of the PNP transistor currnet is going from emitter to input, so we consider r<sub>b'e</sub> for both transistors in the differential pair:

**R<sub>in</sub> = 2 · r<sub>b'e</sub> = 2 · β · r<sub>eb'</sub>**

## CMRR 

CMRR (Common-Mode Rejection Ratio) is the ability of an amplifier to reject noise that appears equally on both inputs. Formally, it is defined as the ratio between the differential gain (A<sub>diff</sub>) and the common-mode gain(A<sub>cm</sub>) and that mean we would like to have it as huge as it's posible and the best solution is to provide large differential gain.

**CMRR = 20 · log(A<sub>diff</sub>/A<sub>cm</sub>)**

## Load of differential pair

For a differential pair, even with a simple implementation, you can identify three main variants of loading:
a) Unbalanced resistive load
b) Balanced load using resistors
c) Balanced load with a current mirror

In our case, the pair will eventually be loaded by the next stage (VAS), which introduces imbalance between the branches. This makes solution (a), using equal resistors in both collectors, a less attractive choice.

Circuit (b) improves the situation by using a single resistor to balance the load, but this is still only a passive compensation method.
The best solution is (c) — the current mirror load — because the transistor in the unloaded branch biases the other transistor, forcing an identical current in the loaded branch. This ensures excellent balance and better performance. Moreover, method (c) offers the best linearity among the three.

<img width="627" height="355" alt="image" src="https://github.com/user-attachments/assets/6cee3803-1442-45cd-a515-cc2c1565ee9d" />

## Slew rate

Slew rate is a parameter that defines how quickly an amplifier can change its output voltage, effectively determining how fast it can turn the next stage on or off.

**Slew rate = dVout/dt = Iss/CL

For a differential pair with a current mirror load, the concept is easier to explain:

- When discharging the load, we assume that transistor M2 is fully on and M1 is off. In that case, the entire tail current from the pair’s current source flows through M2 to ground.

- Conversely, when charging the load, the process also happens with a current equal to the current source value.

Differential pairs with resistive loads typically have an asymmetrical slew rate, because the load resistor diverts part of the current, reducing the effective charging or discharging capability.

<img width="501" height="219" alt="image" src="https://github.com/user-attachments/assets/4b3084d9-0bf7-4412-a156-82c576edb490" />

# Summary

For final design i used combination below:

<img width="414" height="776" alt="image" src="https://github.com/user-attachments/assets/aabd021c-c49b-4b51-92bd-2afab52c0369" />

If you are using a common differential pair with a current mirror load, there is not much you can adjust beyond selecting the transistors and setting the operating point via the current source. I chose the transistors based on their maximum V<sub>CE</sub> (50 V) for safety, and then balanced cost against noise performance. If you are more precise, you can look at β parameter which is mainly relevant for the input pair, since a higher value ensures greater input impedance.

As the current source, I used a configuration with a transistor biased by diodes and a resistor. I also added a potentiometer to allow fine-tuning if needed, and a capacitor Cf1 for filtering, because some AC signal was leaking into the bias voltage.

Initially, I started with 2.5 mA from the source, but after building the whole configuration I decided to reduce it to 1.88 mA.
Keep in mind that this current not only sets the operating point but also affects your gain — and, more importantly, your slew rate.

As mentioned earlier, slew rate defines how fast your circuit can respond. If it’s too slow, the amplifier can start cutting off higher audio frequencies. If it’s too fast, you risk turning your design into an oscillator due to the effects of negative feedback.

Bibliography:
1. Douglas Self, Audio Power Amplifier Design Handbook 4th edition, Elsevier, 2006
2. R. J. Baker, Circuit Design, Layout, and Simulation 3th Edition, Wiley, 2010 
