## Input stage configurations

For the input stage, the most common solution is a differential pair. The main advantage of this configuration is the ability to apply negative feedback, which can be used to compensate for various non-idealities. To keep the design simple, I implemented a basic differential pair with a current mirror as the active load, along with one improvement. I believe this setup will be sufficient for the goals of this project and enought.

## JFET vs BJT

In audio design books, the best choice for input stages are.. BJT. The main reasons are their higher linearity, greater gain, and better temperature stability compared to FETs. The only real drawback of BJTs is the presence of base current, which reduces input impedance. However, in most cases, this trade-off is worth it due to the other significant advantages.

Since my VAS transistor will be NPN, it's generally a good idea to use the opposite type for the input differential pair to ensure proper current flow and biasing. I chose BC556B because it's cheap, has decent gain, and produces low noise plus should withstand a voltage around 50V. And since this is a through-hole design, I can always swap the transistor later if needed.

For load mirror and VAS I used from same family NPN BC557.

## Current source

As current source I used this configuration:

<img src="https://github.com/user-attachments/assets/8c33b94b-ea38-443c-873c-456c7c9793ff" width="550" height="400">

This setup gives me around 2.34 mA. If I need more, I can simply reduce R2. It's a really simple solution, but it has one drawback, current flows continuously through R1.

# Analisys of design

## DC

![image](https://github.com/user-attachments/assets/1b2819e3-61c3-4966-965a-6cf3969fbc8d)
![image](https://github.com/user-attachments/assets/0156c188-108b-4c87-997a-081deb0d2183)



I’d like to explain the concept of source degeneration. It involves adding a resistor to the source/emitter of a transistor, which creates local negative feedback. As a result, the source voltage starts to follow the gate/base voltage more closely. This feedback mechanism reduces the transistor’s gain, but improves linearity and stability. In practice, it makes it harder for the transistor to enter non-linear regions of operation, which can improve THD value.

## Degradation of load:

For load without resistor we can get THD = 0.0204%. After implement resistor we can get this table:

| Resistor [Ω] | THD [%]   |
|--------------|-----------|
| 100          | 0.0182    |
| 200          | 0.0188    |
| 400          | 0.0205    |
| 600          | 0.0198    |
| 800          | 0.0186    |
| 1000         | 0.0221    |

As you can see for 100Ω and 200Ω we got less that orginal value, then THD is oscilating around 0.02%. For low value of resitor we can see the benefits of degeneration, but for higher resistacne the gain drops and despite Q5 and Q4 works more linear, the whole circuit loses THD by low gain (look at chart).

Chart of gain for difrent resistors:

![image](https://github.com/user-attachments/assets/c883ae25-4699-4cb0-8647-4089f6ca7cd8)

I found sweet point for 220Ω (series E6) value and got around 0.0174% of THD.

# Degradation of differential pair:
