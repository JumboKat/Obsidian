![[Pasted image 20260730172129.png]]
A MOSFET (*Metal-Oxide-Semiconductor Field-Effect Transistor*) is a component used to switch or amplify electrical signals. Every MOSFET has three terminals: **gate**, **drain**, and **source**, although some may have these in a different order, which is specified by the manufacturer's datasheet.

![[Pasted image 20260730172503.png]]
The part number, which is printed on the front, can be used to find the component's datasheet online.

![[Pasted image 20260730172322.png]]
In electrical diagrams, they are represented by the symbols above.
### How a MOSFET Functions
![[Pasted image 20260802155230.png]]
A MOSFET works as a switch or current amplifier, depending on the voltage received by the **gate** terminal. In a circuit, the **drain** acts as the negative terminal, while the **source** as the positive. If no voltage is applied to the gate terminal, the MOSFET will act as an open switch, preventing electrons from flowing through. If voltage is applied, however, current will be allowed to flow. Additionally, varying the voltage received by the gate pin will vary the current passing through. As an example, if the MOSFET is used to control the operation of a lamp, it can be used to switch it on and off, as well as dim its brightness by controlling the power it receives. A controller or sensor can be used to control the lamp automatically.\

MOSFETs can be turned on and off very fast using a **pulse width modulation (pwm)** signal. 
#### Managing Heat
![[Pasted image 20260804110613.png]]
High current can cause a MOSFET to heat up, so they are often attached to heat sinks, which increase the surface area for heat dissipation.
### MOSFET vs BJT
A BJT (*bipolar junction transistor*) transistor and a MOSFET play the same role in a circuit. The main difference is that a BJT requires current to be applied to the base pin, whilst a MOSFET only requires voltage to be applied to its gate pin, making the circuit more efficient and simpler in design. A MOSFET can also handle much more current than a BJT.