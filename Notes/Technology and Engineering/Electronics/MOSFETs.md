### What is a MOSFET?
![[Pasted image 20260730172129.png]]
A MOSFET (*Metal-Oxide-Semiconductor Field-Effect Transistor*) is a component used to switch or amplify electrical signals. Every MOSFET has three terminals: **gate**, **drain**, and **source**, although some may have these in a different order, which is specified by the manufacturer's datasheet.

![[Pasted image 20260730172503.png]]
The part number, which is printed on the front, can be used to find the component's datasheet online.
#### MOSFET Forms
They come in both through-hole form (larger, used on test boards and PCBs) and SMD (*surface mounted device*) form (used in compact electronics).

![[Pasted image 20260804194947.png]]
Multi-pin MOSFETs combine multiple units into one device.
### Using MOSFETs
![[Pasted image 20260802155230.png]]
A MOSFET works as a switch or current amplifier, depending on the voltage received by the **gate** terminal. In a circuit, the **drain** acts as the negative terminal, while the **source** as the positive. If no voltage is applied to the gate terminal, the MOSFET will act as an open switch, preventing electrons from flowing through. If voltage is applied, however, current will be allowed to flow. Additionally, varying the voltage received by the gate pin will vary the current passing through. As an example, if the MOSFET is used to control the operation of a lamp, it can be used to switch it on and off, as well as dim its brightness by controlling the power it receives. A controller or sensor can be used to control the lamp automatically.

MOSFETs can be turned on and off very fast using a **pulse width modulation (pwm)** signal. 
#### Managing Heat
![[Pasted image 20260804110613.png]]
High current can cause a MOSFET to heat up, so they are often attached to heat sinks, which increase the surface area for heat dissipation.
#### MOSFET vs BJT
A BJT (*bipolar junction transistor*) transistor and a MOSFET play the same role in a circuit. The main difference is that a BJT requires current to be applied to the base pin, whilst a MOSFET only requires voltage to be applied to its gate pin, making the circuit more efficient and simpler in design. A MOSFET can also handle much more current than a BJT.
### Example Circuits
#### Floating Gate Problem
![[Pasted image 20260804195305.png]]
In this example circuit, a lightbulb is connected to one end of a 9V battery. The other end of the lightbulb connects to the drain of a MOSFET, while the other end of the battery connects to the source. The gate is connected to an open wire. In this state, the lightbulb is off.

![[Pasted image 20260804195555.png]]
If the gate wire is connected to the 9V supply, the MOSFET is activated and the lightbulb is turned on. If we now disconnect the gate pin, the lightbulb remains on; the gate acts like a capacitor, storing charge. The charge has nowhere to go when the wire is disconnected. We can connect the gate wire to ground to disconnect it.
#### Gate Protection 
![[Pasted image 20260804200028.png]]
Normally, there is no current at the gate terminal. However, when the MOSFET switches from off to on, electrons rush into the gate terminal in an instant, causing a spike in current. A device controlling the MOSFET like an arduino or microcontroller can be damaged by the flux in current.

![[Pasted image 20260804200809.png]]To limit the current, a resistor should be used between the source and the gate terminal. To avoid the floating gate problem, a path to ground should be provided, however simply adding a wire would create a short circuit, resulting in too much current flowing through. A pull down resistor should be placed to limit the current as the MOSFET is discharged.
![[Pasted image 20260804200954.png]]
Placing the pull down resistor on the left side is usually preferred, as placing it on the right would create a voltage divider, resulting in less voltage reaching the gate pin.
#### Dimming with a Potentiometer
![[Pasted image 20260804201244.png]]
A potentiometer can be used in conjunction with a MOSFET to control the brightness of a lightbulb in a circuit. The potentiometer, in series with a resistor, is connected to the gate. Here, the resistance is so high that the current is negligible, and only a voltage is received. Disconnecting the power to the resistor (and hence breaking the circuit) causes the MOSFET to drain to ground through the potentiometer. 

While connected, turning the potentiometer to increase its resistance will decrease the voltage at the gate. As the resistance is increased, the MOSFET begins to deactivate, and the brightness of the lightbulb dims, until it eventually turns off.
### How MOSFETs Work
#### Enhancement vs. Depletion
![[Pasted image 20260804201918.png]]
There are two types of MOSFET, which are distinguished by their operation state when no voltage is applied to the gate: **enhancement** (which is off by default) and **depletion** (which is on by default). 

![[Pasted image 20260730172322.png]]
Physically, the two types look nearly identical. On electrical diagrams, the enhancement type is represented with a broken line across the terminals, whereas the depletion type has its terminals in a continuous line. 
Both come in either N-channel (*NMOS*) or P-channel (*PMOS*) versions, indicated by the direction of the arrow. NMOS and PMOS transistors differ in how they are activated (turning on for enhancement type and off for depletion type); an NMOS is activated by a positive gate-to-source voltage (Vg > Vs), while a PMOS is activated by a negative gate-to-source voltage. "Activation" is achieved by increasing or decreasing the resistance of the drain-to-source channel.

![[Pasted image 20260805163650.png]]
The drain terminal is attached to a copper plate with a semiconductor soldered onto it. From this semiconductor are two wires that attach to the gate and source. The semiconductor is usually made of silicon, whose atoms have four valence electrons. Thus, on their own, they will share electrons with each other via **covalent bonds**. 
#### PN Junctions
![[Pasted image 20260805164043.png]]
If we inject phosphorus atoms (called "**doping**"), which have five valence electrons, four of valence electrons will form covalent bonds with silicon atoms, and the fifth will be able to move around freely. Since electrons are negatively charged, this is called **N-type doping**; the material now has an overall negative charge, and is known as an **N-type material**.

If we inject Boron atoms (which have 3 valence electrons), they won't have enough electrons to satisfy all of their neighbors, resulting in a "hole" of positive charge, resulting in a **P-type material**.

![[Pasted image 20260805164704.png]]
When an N-type and P-type material meet, a **PN junction** forms between them, where a depletion region occurs; some electrons will move to the P side, while some "holes" move to the N side. This creates an electric field, which acts as a barrier preventing more electrons or holes from moving across.
#### The MOSFET Design (N-channel enhancement type)
![[Pasted image 20260806180120.png]]
For an N-channel enhancement type MOSFET, a P-type silicon forms the base layer, with two small regions of N-type material. At the junctions between the two materials, a depletion region forms. The drain terminal connects to one N-type region while the source connects to the other. The **body** metal plate connects to the underside of the silicon substrate and to the source (in electrical diagrams, the symbol shows the base terminal directly connected to source). A layer of silicon dioxide (*SiO<sub>2</sub>*) sits between the source and drain and between the metal plate that represents the gate terminal. 

![[Pasted image 20260806180951.png]]The silicon dioxide is an insulator with extreme resistance, separating the gate from the silicon semiconductor base and prevents current from flowing through. It is also a dielectric; if a positive charge is induced on one side, negative charges build up on the other side, forming an electric field between them, similar to a capacitor.

The breakdown for the meaning behind MOSFET is as follows:
- **M**etal: originally referred to the metal plate that served as the gate (though today other conductive materials are used instead of pure metal).
- **O**xide: the thin insulating layer (silicon dioxide) between the gate and the semiconductor body
- **S**emiconductor: the silicon base material where the current passes through.
- **F**ield-**E**ffect: the electric field formed by sending voltage to the gate controls the current through the channel (as opposed to current directly driving the base like in a BJT).
- **T**ransistor: describes the role of a MOSFET: a semiconductor device used to switch or amplify signals.

When current is applied across the MOSFET (with no voltage to the gate), no current will flow. Between source and drain, current must flow through the P-type substrate to get from one N-type region to the other. This structure is essentially two PN junctions placed together back-to-back, meaning one of the two will always be reverse-biased no matter which way the current flows.
![[Pasted image 20260901193429.png]]
When a positive voltage is applied to the gate pin, an electric field forms that penetrates through the silicon dioxide and into the conductor substrate, making it act like a capacitor.

![[Pasted image 20260908105834.png]]
As the voltage on the gate increases, more and more electrons are attracted to it, but can't pass through the silicon dioxide barrier. Holes in the region are filled or pushed away. 

![[Pasted image 20260908110040.png]]
Eventually, the build-up of electrons at the surface creates a negatively charged channel, (hence the name N-channel MOSFET). This region is now essentially of N-type material and forms a continuous bridge between source and drain. If voltage is taken away, the electrons disperse and current stops flowing. The voltage must also reach a certain voltage, called the **threshold voltage** to properly form the connection, else the bridge is incomplete and current does not flow. 

![[Pasted image 20260908111914.png]]
The symbol for an N-channel enhancement type MOSFET shows an arrow pointing towards the gate, indicating that electrons are being pulled up to form the channel.
#### Channel Width
Increasing the gate voltage widens the channel, increasing current. The bridge is maintained by the voltage difference between the gate and the bridge at any point along its length. (The higher relative voltage, the more electrons are pulled toward the gate).
![[Pasted image 20260908111020.png|340]]![[Pasted image 20260908110956.png|346]]
As the drain-to-source voltage increases, the resistance decreases, causing the current to increase linearly. This takes place in the **ohmic region**. However, as voltage increases, the channel narrows at one end as the voltage difference between the gate and drain decreases. The channel narrows as voltage increases until the **pinch off point**.

![[Pasted image 20260908111630.png]]
Beyond the pinch off point is the **saturation region**, where the channel can't fully close, but the current can't increase anymore. Only increasing the gate voltage will increase the current.
#### N-channel Depletion Type
The depletion type is similar to the enhancement type, except that in its default state, the source and drain are physically connected by a thin N-type layer, facilitating current flow.

![[Pasted image 20260908112207.png]]
As negative voltage is applied to the gate, electrons in the substrate are pushed away while holes are attracted. This narrows the channel until, when enough negative voltage is applied, the channel becomes blocked. However, applying a positive voltage to the gate will have the reverse effect, widening the channel and increasing current flow.
#### P-channel Enhancement Type
![[Pasted image 20260908112412.png]]
A P-channel enhancement type is off by default, with an N-type substrate and an isolated P-type region at both the source and drain. As a negative voltage is applied to the gate, electrons are repelled while holes are attracted, creating a positive bridge linking the two P-type regions, allowing current to flow. 
#### P-channel Depletion Type
![[Pasted image 20260908112653.png]]
A p-channel depletion type is on by default, with the two P-type regions physically connected by a P-type bridge. When positive voltage is applied to the gate, electrons are attracted while holes are pushed away, narrowing the channel until it is eventually blocked. Thus, we can turn it off by applying a positive voltage. However, applying a negative voltage has the opposite effect.

---
### References
![](https://www.youtube.com/watch?v=AwRJsze_9m4&list=PLWv9VM947MKi8KxDEki9x0FkQv3WBb2Ev)