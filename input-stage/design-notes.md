## Input stage configurations

For the input stage, the most common solution is a differential pair. The main advantage of this configuration is the ability to apply negative feedback, which can be used to compensate for various non-idealities. To keep the design simple, I implemented a basic differential pair with a current mirror as the active load, along with one improvement. I believe this setup will be sufficient for the goals of this project. I think it will be enought for this project.

## JFET vs BJT

In audio design books, the best choice for input stages are.. BJT. The main reasons are their higher linearity, greater gain, and better temperature stability compared to FETs. The only real drawback of BJTs is the presence of base current, which reduces input impedance. However, in most cases, this trade-off is worth it due to the other significant advantages.

Since my VAS transistor wiil be NPN, it's generally a good idea to use the opposite type for the input differential pair to ensure proper current flow and biasing. I chose BC556B because it's cheap, has decent gain, and produces low noise. And since this is a through-hole design, I can always swap the transistor later if needed.
