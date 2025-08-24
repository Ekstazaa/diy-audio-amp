# VAS - Voltage Amplifier Stage

The Voltage Amplification Stage (VAS) is a crucial part of any audio power amplifier. Its main role is to amplify the small signal coming from the input stage to a level close to the full supply voltage swing, preparing it to drive the output stage. In other words, the VAS is responsible for faithfully reproducing the input waveform but with a much larger amplitude.

Because of its key role, the operating point of the VAS transistor is extremely important. Poor biasing or incorrect design can lead to severe non-linearity, distortion, and reduced bandwidth.

For beginners, the most common and practical implementation of the VAS is the common-emitter amplifier. This topology provides high voltage gain, good linearity (when properly biased), and simplicity, making it the standard starting point for amplifier design.

## Common Emitter configuration

<img width="469" height="346" alt="image" src="https://github.com/user-attachments/assets/877980bc-a2fd-4963-add7-59d154d1e30a" />

The circuit shown above is the most classic and academic transistor configuration, making it a great starting point.

Its operation is simple to understand: when the input voltage rises, the NPN transistor begins to conduct heavily, effectively pulling the collector toward ground — the output drops.
When the input voltage decreases, the transistor enters cutoff, and the output approaches the supply voltage.
If we interpret the signals as logic states, this stage acts as an inverter, introducing a 180° phase shift between input and output.

Because of the transistor’s current amplification, a relatively small change in input voltage can produce a large change at the output. This is the foundation of its high voltage gain, making the common-emitter stage a key building block in amplifiers.

### Operating point

### Gain

### Input Impednace

### Output Impedance
