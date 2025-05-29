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

## Theory of shicali

Let's look at this configuration:

![image](https://github.com/user-attachments/assets/a2561511-7f27-4a94-a2d2-6a0555689756)

The Shiklai pair is made of a power transistor (Q1) and control transistor (Q2). The main idea is to make the vout closely follow vin. When vin increases, Q2 starts to conduct more, which means that the base current of Q1 also increases. As a result, the output current delivered to the load goes up, causing vout to rise, so as to ensure a constant voltage drop across Q2's Vbe.

Sziklai pair behaves like a Darlington configuration, but with local negative feedback, which improves linearity and control.

In that configuration i used also ressistor R1. It helps to turn off/on Q1 by creating a voltage drop on Q1's Veb faster. In theory, the resistor should be as small as possible, but a balanced value is needed. If it's too small, it can lead to instability and reduced linearity due to excessive loop gain and insufficient damping.

## Simulations

Screenshot of testbench:
![image](https://github.com/user-attachments/assets/5c2e29cd-9dc6-40d9-a26d-e5a6b267129a)





bibliography:
1. Douglas Self, Audio Power Amplifier Design Handbook 4th edition, Elsevier, 2006
2. Rod Elliott, Compound Pair Vs. Darlington Pairs, https://sound-au.com/articles/cmpd-vs-darl.htm, 2011
