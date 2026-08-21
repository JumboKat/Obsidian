There are two main types of transistors: **BJTs** (*Bipolar Junction Transistors*) and Field Effect. They are small electronic components with two main functions: acting as a switch, and amplifying signals.

![[Pasted image 20260820163502.png]]
Low power transistors have a resin case, while higher power transistors will have a case made of resin and metal; the metal part is usually attached to a heat sink to dissipate heat.
### Reading Transistors
![[Pasted image 20260820163724.png]]
On the face of the case, a transistor will have its part number used to identify it and find its datasheet, which is important for knowing voltage limits.
### Function
![[Pasted image 20260820163837.png]]
A transistor has three pins: **E**mitter, **B**ase, and **C**ollector. Typically, for a transistor with a flat edge, the left pin is the emitter, the middle is the base, and the right is the collector, though this may not always be the case.

![[Pasted image 20260820164639.png]]
A transistor can be used as a switch. Only when a voltage is applied to the middle base pin can current flow from collector to emitter (conventionally). By controlling the voltage to the base, the opening and closing of the circuit can be automated or remotely triggered. 

At least **0.6-0.7V** needs to be applied to the base to turn the transistor on. The more voltage that is applied, the more current is allowed to flow. A small change in voltage causes a large change in the main circuit; hence, the transistor acts as an amplifier.
### Current Gain
![[Pasted image 20260820170751.png]]
The base pin typically has a very low current relative to the collector current. The ratio between these two, represented by β, is called the **current gain**.
### NPN and PNP Types
While there are two types of BJT transistors, they visually look identical; they can be distinguished by checking their part number.
#### NPN Transistor
![[Pasted image 20260820202308.png]]
In an NPN transistor, the current flowing through the collector and the base *combine* to flow out of the emitter.
#### PNP Transistor
![[Pasted image 20260820202515.png]]
In a PNP transistor, the emitter connects to the positive; current flows into the emitter and *divides* as it flows out through the base and collector terminals.

![[Pasted image 20260820202626.png]]
Transistors are represented by the above symbols in circuit diagrams. There is an arrow at the emitter which indicates the direction of conventional current.