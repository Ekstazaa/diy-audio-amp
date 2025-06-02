## Input stage configurations

For the input stage, the most common solution is a differential pair. The main advantage of this configuration is the ability to apply negative feedback, which can be used to compensate for various non-idealities. To keep the design simple, I implemented a basic differential pair with a current mirror as the active load, along with one improvement. I believe this setup will be sufficient for the goals of this project. I think it will be enought for this project.

## JFET vs BJT

In audio design books, the best choice for input stages are.. BJT. The main reasons are their higher linearity, greater gain, and better temperature stability compared to FETs. The only real drawback of BJTs is the presence of base current, which reduces input impedance. However, in most cases, this trade-off is worth it due to the other significant advantages.

Since my VAS transistor will be NPN, it's generally a good idea to use the opposite type for the input differential pair to ensure proper current flow and biasing. I chose BC556B because it's cheap, has decent gain, and produces low noise plus should withstand a voltage around 50V. And since this is a through-hole design, I can always swap the transistor later if needed.

For load mirror and VAS I used from same family NPN BC557.

## VAS

VAS is just voltage ampifier stage. Initially, I planned to analyze the input differential pair on its own. However, as I progressed, I realized that adding a realistic load would provide more meaningful results. Additionally, since the VAS introduces a 180-degree phase shift, including negative feedback makes the setup more representative of the final circuit, allowing for a more accurate analysis.

Moving forward, for the VAS I used a simple common-emitter configuration with a current source on the collector and diodes for biasing the output stage. It's not complicated and provides decent gain. There were some issues with phase margin at first, but adding a Miller capacitor was enough to stabilize the circuit.

## Current source

As current source I used this configuration:

<img src="https://github.com/user-attachments/assets/8c33b94b-ea38-443c-873c-456c7c9793ff" width="550" height="400">

This setup gives me around 2.34 mA. If I need more, I can simply reduce R2. It's a really simple solution, but it has one drawback, current flows continuously through R1.

## Analisys of design

Testbench

![image](https://github.com/user-attachments/assets/b3637ef1-33c9-4390-adb2-ba878bb8a7a2)

Parameters for close loop as follower:
Verror = 21.24uV
Vout range = <-23.94V; 23.82V>

Max pssr for BW from 20Hz to 20kHz for close loop as follower:
PSRRvdd = -124.15dB
PSRRvss = -111.34dB

AC analise in open loop:
Adc = 109.14dB
f0 = 14.3MHz
f3dB = 51.137Hz
PM = 60.105deg
GM = 11.63dB

![image](https://github.com/user-attachments/assets/d7b71830-cc5c-4249-b99d-0557f77e5068)








