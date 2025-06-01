# **Output Stage**

## Test Conditions

All key measurements and simulations of the output stage will primarily be conducted under the following conditions:

- **Signal Frequency:** 1kHz and 20kHz  
- **Input Amplitude:** 20V peak-to-peak (Vpp)
- **Load Impedances:** 32 Ω and 64 Ω

These parameters were chosen as they represent a typical use-case scenario in audio applications and allow clear observation of linearity, distortion, and thermal behavior.

Based on the book written by Douglas[1], I will be testing this block based on specific parameters:

- **Total Harmonic Distortion (THD)**  
- **Power efficiency**
- **Noise**

## Output Stage Configurations

Before getting into the test results, I want to briefly go over the most common output stage configurations found in audio amplifiers. The two that come up the most are:

- **CFP – Complementary Feedback Pair (also known as the Sziklai pair)**  
- **EF – Emitter Follower (also known as the Darlington pair)**

Both of these topologies actually come from earlier **quasi-complementary designs**, which were developed when good PNP power transistors weren’t widely available. Nowadays, since complementary NPN/PNP devices are easy to get, using full complementary symmetry just makes more sense.

![image](https://github.com/user-attachments/assets/0672a328-6fee-4d1b-961b-1b9a8c8c6d06)

For this project, I decided to use the **CFP configuration**. After reading Rod Elliott’s article [2], I got the impression that it’s generally better suited for audio amplifiers. It tends to produce less distortion than other topologies. Of course, the actual difference between CFP and EF is often small and usually not audible unless you really know what to listen for — and even then, it can be purely subjective.

Still, from a design perspective, the CFP offers better linearity and thermal behavior, and that was enough for me to go with it.

To wrap up the topic of output stage topologies, I should also mention triple configurations, which basically just add more transistors to the chain. From what I’ve read, these are mostly used in commercial designs, where high current efficiency is a priority. As you can probably guess, that kind of complexity isn’t really necessary for this DIY project.

## Theory of shiklai

Let's look at this configuration:

<img src="https://github.com/user-attachments/assets/a2561511-7f27-4a94-a2d2-6a0555689756" width="500" height="600">

The Shiklai pair is made of a power transistor (Q1) and control transistor (Q2). The main idea is to make the vout closely follow vin. When vin increases, Q2 starts to conduct more, which means that the base current of Q1 also increases. As a result, the output current delivered to the load goes up, causing vout to rise, so as to ensure a constant voltage drop across Q2's Vbe.

Sziklai pair behaves like a Darlington configuration, but with local negative feedback, which improves linearity and control.

In that configuration i used also ressistor R1. It helps to turn off/on Q1 by creating a voltage drop on Q1's Veb faster. In theory, the resistor should be as small as possible, but a balanced value is needed. If it's too small, it can lead to instability and reduced linearity due to excessive loop gain and insufficient damping.

## Simulations

In the testbench, I used the same current source that will be implemented in the final amplifier design to make the simulation results more realistic. The circuit was tested with a ±10 V, 1 kHz input signal, and the analysis covered 10 cycles. A 32 Ω resistor was used as the load:

Screenshot of testbench:

<img src="https://github.com/user-attachments/assets/ef2e7075-d2fc-4220-9793-0461b7ba6d50" width="800" height="700">

The two main things I can tweak are the transistors in the Sziklai pair and the resistors. For the control transistors, I chose BC556B and BC557B, mainly to keep the design coherent — differencial pair, current soures and VAS I used the same.

As for the power transistors, I went with 2SA1943 and 2SC5200. I picked these mostly based on cost and availability. Honestly, I don’t have much experience with selecting "ideal" transistors yet, so I just chose the most common and affordable ones I could find.

After analyzing the transistors, I tested the influence of resistors R1 and R2 by sweeping their values in LTspice.

| R [Ω] | THD [%] | AVG(i(vup)) [mA] | AVG(i(vdn)) [mA] |
|-------|---------|------------------|------------------|
| 100   | 0.0885  | 108.936          | -88.531          |
| 200   | 0.0689  | 109.449          | -89.033          |
| 300   | 0.0808  | 111.864          | -91.442          |
| 400   | 0.0808  | 113.779          | -93.353          |
| 500   | 0.0775  | 115.219          | -94.790          |
| 600   | 0.0753  | 116.255          | -95.824          |
| 700   | 0.0724  | 117.143          | -96.712          |
| 800   | 0.0704  | 117.841          | -97.408          |
| 900   | 0.0676  | 118.486          | -98.054          |
| 1000  | 0.0659  | 119.007          | -98.575          |
| 2000  | 0.0554  | 122.078          | -101.644         |
| 3000  | 0.0513  | 123.594          | -103.158         |
| 4000  | 0.0492  | 124.566          | -104.128         |
| 5000  | 0.0476  | 125.261          | -104.827         |
| 10000 | 0.0439  | 127.274          | -106.830         |

As a reminder, the resistor is used to accelerate the switching of the power transistors. The chart shows that when the resistance is high, the circuit struggles to turn off transistors Q1 and Q3. For values around 10 kΩ and higher, the transistors remain on throughout the entire cycle and operate in saturation. This indicates that the circuit behaves like a Class A amplifier.

Additionally, the average current through the branch increases with higher resistance values. To reduce power consumption, lower resistor values are needed. However, this comes with a trade-off: reducing the resistance increases THD and decreases linearity (you can see the breakdown in the characteristic). As always, it’s a matter of compromise.

I initially spent a lot of time analyzing that configuration for myself, but in practice, using 100Ω, 200Ω, or 300Ω doesn't really make much of a difference, since the resistance will drift with temperature regardless. The safest option is to choose a 330 Ω resistor from the E6 series.

![image](https://github.com/user-attachments/assets/677a652e-87c0-4563-81ee-60e71a3d1417)

Bibliography:
1. Douglas Self, Audio Power Amplifier Design Handbook 4th edition, Elsevier, 2006
2. Rod Elliott, Compound Pair Vs. Darlington Pairs, https://sound-au.com/articles/cmpd-vs-darl.htm, 2011
