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

![image](https://github.com/user-attachments/assets/a2561511-7f27-4a94-a2d2-6a0555689756)

The Shiklai pair is made of a power transistor (Q1) and control transistor (Q2). The main idea is to make the vout closely follow vin. When vin increases, Q2 starts to conduct more, which means that the base current of Q1 also increases. As a result, the output current delivered to the load goes up, causing vout to rise, so as to ensure a constant voltage drop across Q2's Vbe.

Sziklai pair behaves like a Darlington configuration, but with local negative feedback, which improves linearity and control.

In that configuration i used also ressistor R1. It helps to turn off/on Q1 by creating a voltage drop on Q1's Veb faster. In theory, the resistor should be as small as possible, but a balanced value is needed. If it's too small, it can lead to instability and reduced linearity due to excessive loop gain and insufficient damping.

## Simulations

Screenshot of testbench:
![image](https://github.com/user-attachments/assets/4c06d760-c8f7-4eee-a232-570ad15c55c2)

In this configuration, the two main things I can tweak are the transistors in the Sziklai pair and the resistors. For the control transistors, I chose BC556B and BC557B, mainly to keep the design coherent — differencial pair, current soures and VAS I used the same.

As for the power transistors, I went with 2SA1943 and 2SC5200. I picked these mostly based on cost and availability. Honestly, I don’t have much experience with selecting "ideal" transistors yet, so I just chose the most common and affordable ones I could find.

After analyzing the transistors, I tested the influence of resistors R1 and R2 by sweeping their values in LTspice.

| R [Ω] | THD [%] | Idn_avg [mA] |
|-------|---------|--------------|
| 100   | 0.109   | 98.32        |
| 200   | 0.091   | 111.81       |
| 300   | 0.073   | 118.35       |
| 400   | 0.060   | 122.52       |
| 500   | 0.050   | 125.58       |
| 600   | 0.043   | 128.03       |
| 700   | 0.037   | 130.16       |
| 800   | 0.032   | 131.92       |
| 900   | 0.030   | 133.34       |
| 1000  | 0.027   | 134.51       |
| 2000  | 0.034   | 140.70       |
| 4000  | 0.039   | 143.84       |

Up to around 1kΩ, the THD decreases noticeably, which is clearly visible in the waveform plots. You can see that for values below 700Ω, the sinewave becomes visibly distorted, confirming the theory that a too small resistor in this configuration reduces linearity. On the other hand, if the resistance increases, we must give more average current flowing through the pair - this means a certain compromise. It is worth noting that for small resistances (100-300Ω) the current changes are more sensitive.

Moreover, for values above 1 kΩ, THD is rising back, because the Sziklai pair remains in saturation throughout the entire signal cycle, effectively making the amplifier operate in class A.
The reason for this is that the pair becomes too slow — the driver transistor is unable to discharge the power transistor’s base quickly enough, keeping it continuously active.

Overall, I think the best option is to choose a 470 Ω resistor (E6 series), because we're in the less sensitive region of the Idn_avg(R) curve. It also gives us some thermal headroom — if the resistance increases due to temperature, it's still safe — and THD remains at a good level. Anyway if there is a problem on the pcb board, it will not be a difficult change. 

![image](https://github.com/user-attachments/assets/eb58f046-b7df-4503-b0d3-8a784669f3da)

At the end 

Bibliography:
1. Douglas Self, Audio Power Amplifier Design Handbook 4th edition, Elsevier, 2006
2. Rod Elliott, Compound Pair Vs. Darlington Pairs, https://sound-au.com/articles/cmpd-vs-darl.htm, 2011
