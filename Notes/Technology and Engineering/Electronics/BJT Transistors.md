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
#### PN-Junctions
![[Pasted image 20260824093655.png]]
P-type and N-type materials are combined to form NPN or PNP transistors with two [[Diodes#How Diodes Work|PN Junctions]] between them. These junctions naturally prevent current from flowing through them.

![[Pasted image 20260824095029.png]]
In an NPN transistor, the emitter is heavily doped with many excess electrons, the base is lightly doped with only a few holes, and the collector is moderately doped, with some excess electrons.

![[Pasted image 20260824095318.png]]
The base layer is kept thin and lightly doped on purpose to create as few holes as possible. This means that only a small amount of electrons combine with holes in the base, while the majority are able to cross the barrier. The negative terminal pushes electrons toward the emitter, while the positive end pulls electrons away from the base. This is how a BJT works as an amplifier; a small amount of current in the base terminal allows for a much larger current from the emitter to the collector. 

![[Pasted image 20260824095804.png]]
When the collector is connected to the positive, the electrons (and the holes) in the collector are pulled toward the positive terminal, causing a reverse bias. While this keeps the electrons in the collector from crossing to the P-type side, the electrons in the P-type side are pulled to the holes in the collector, while the holes in the collector are pulled to the base. Because of the excess of electrons in the emitter, these will occupy the holes in the base, and because of the electric field pointing from the collector to the base, some of these will be pulled across, developing a current across the reverse bias junction. A higher voltage at the base pin means more electrons passing through the emitter while relatively few combine in the base.

---
### References
![](https://www.youtube.com/watch?v=J4oO7PT_nzQ&list=PLWv9VM947MKi8KxDEki9x0FkQv3WBb2Ev&index=5)