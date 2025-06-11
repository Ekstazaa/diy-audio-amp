## Input stage configurations

For the input stage, the most common solution is a differential pair. The main advantage of this configuration is the ability to apply negative feedback, which can be used to compensate for various non-idealities. For this chapter a will explain basic work of differential pair, parameters and some improvements.

## JFET vs BJT

In audio design books, the best choice for input stages for power amplifiers are.. BJT. The main reasons are their higher linearity, greater gain, and better temperature stability compared to FETs. The only real drawback of BJTs is the presence of base current, which reduces input impedance. However, in most cases, this trade-off is worth it due to the other significant advantages.

# Differential pair

![image](https://github.com/user-attachments/assets/726ff399-3be6-479d-9154-1d076e55de30)

A differential pair is a circuit built from two **identical** transistors with same load supplied by a constant current source. The core idea is that the current through each transistor depends on the voltage difference between the two inputs. If both inputs are equal, the current splits evenly — each side conducts I/2. When vin1 is higher than vin2, more current flows through the transistor on the right side what causes higher voltage on vout2 and vice versa. 

![image](https://github.com/user-attachments/assets/5b9465f2-5fcf-4630-8122-23765ab88b20)

At start let's look at the chart above. It shows how the current through resistors R1 and R2 changes depending on the voltage difference between the inputs (note: negative values indicate that vin1 < vin2).
This graph has few conclusions:

1. The characteristic curve assumes perfectly matched transistors, which means the current splits evenly (I/2) when the voltage difference is exactly 0 V. In reality, transistors can have slight mismatches due to manufacturing tolerances, leading to input offset current or voltage, even when vin1 = vin2.
2. The linear operating region of the differential pair is quite narrow (around ±100 mV). This means the input stage is very sensitive, and even a small voltage difference between the inputs can lead to a significant imbalance in output current - in other words differencial pair has huge gain. Outside that range, one transistor begins to dominate completely, pushing the other almost into cutoff.
3. As we move further outside the linear range, one of the transistors starts to dominate entirely, conducting nearly the full bias current, while the other one effectively shuts off. At this point, the circuit loses its linear behavior and enters a region where the output no longer changes proportionally to the input difference. In practical terms, this limits how large the differential input voltage can be **before distortion appears**.

In audio designs, the input signal is usually kept within the linear region, often by using some form of negative feedback or by limiting the gain.

## Gains of differental pair

Just like the differential pair has two inputs, it also has two outputs. Because of that, we can define three different types of voltage gain, depending on how we observe the outputs:
1. Gain measured at vout1:



2. Gain measured at vout2
- And the differential gain, which is the difference between vout1 and vout2


parametry:
- rezystancja wejściowa
- wzmocnienie róznicowe,
- wzmocnienie lewej i prawej gałęzi
- wzmocnienie cmrr
slew rate
obciążenia pary
degradacja pary
inne konfiguracje


## Current source

As current source I used this configuration:

<img src="https://github.com/user-attachments/assets/8c33b94b-ea38-443c-873c-456c7c9793ff" width="550" height="400">

This setup gives me around 2.34 mA. If I need more, I can simply reduce R2. It's a really simple solution, but it has one drawback, current flows continuously through R1.
