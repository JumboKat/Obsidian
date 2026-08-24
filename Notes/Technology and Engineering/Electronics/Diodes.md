![[Pasted image 20260809133504.png]]
A diode is an electrical component that only allows current to flow in one direction. It is comprised of a cylindrical body with a stripe at one end and two metal connectors at each end, with one end being the **anode** and the other the **cathode**. Common specialized diodes include **zener diodes** and **light emitting diodes (LEDs)**.
### How Diodes Work
#### Operation States
![[Pasted image 20260809134046.png]]
To allow current to flow through (achieving **forward bias**), the stripe end of the diode must be connected to the negative, and then other end to the positive. The diode is now operating as a conductor.

To achieve **reverse bias** (block the flow of current) the stripe end must be connected to the positive, and the other end to the negative. The diode is now operating as an insulator.
#### Conductive Materials
![[Pasted image 20260809134723.png]]
Electricity is the flow of free electrons between atoms. Copper is widely used as a conductor since it has many free electrons. Rubber is used as an insulator, since its electrons are held tight and cannot move freely. 

When looking at an atom, it is comprised of a nucleus at its centre with concentric rings of electrons called shells. The outermost shell, the **valence shell**, is where electrons contain the most energy, as the further they are from the nucleus, the more energy they have. The final layer of the atom is the **conduction band**. If electrons can reach this layer, they can become free electrons and move between atoms. 

In a conductor like copper, there are 1-3 electrons in its valence shell. Since atoms tend towards complete shells, the valence shell is more likely to lose its atoms than gain new ones. Furthermore, the valence shell and conduction band overlap, making it very easy for electrons to break free from its atom.

In an insulator, the outermost shell has many electrons, with little room for new electrons. The nucleus has a tight hold on its electrons, and the conduction band is far away. Thus, electrons are very unlikely to escape, and electricity will not flow through this material.

In a semiconductor like silicon, there are one too many electrons in its valence shell to be a conductor (usually four in total), so normally it behaves like an insulator. However, the conduction band is relatively close, and when external energy is received, some electrons in the outermost shell can make the jump to become free. Thus, semiconductors can behave as either a conductor or an insulator.
#### The PN Junction
![[Pasted image 20260809140551.png]]
Within the silicon core of a diode is a P-type material at the anode end, and an N-type material at the cathode end. This is all encased in a protective resin shell.

![[Pasted image 20260809140040.png]]
Pure silicon has very few free electrons; its atoms have exactly four valence electrons, and to achieve eight in total, they 'share' electrons with each other via **covalent bonds**. When an **N-type** material like phosphorous is added (aka "doped"), the atoms form covalent bonds as before, but since phosphorous has five valence electrons, the fifth is able to move freely throughout the material. **P-type** material like aluminum, which only has three valence electrons, there aren't enough electrons to satisfy all four of its neighbors with a full shell, thus resulting in a "hole" that is of positive charge.

![[Pasted image 20260809140752.png]]
The p-type material contains holes for electrons, while the n-type material contains excess electrons. When these two materials are put together, the region between them is called the **PN junction**. Here, some electrons from the negative side move to occupy some holes on the positive side, leaving holes on the positive side. This migration forms a region that is slightly negatively charged on the P side and a region that is slightly positively charged on the N side. These regions, while charged, cannot facilitate electron movement; the N-type atoms who lost an electron, and the P-type atoms who gained an electron, now have their valence shells complete.

![[Pasted image 20260809141054.png]]
Between these slightly charged regions, an electric field forms (created by the ionized atoms in the depletion region; when electrons escape a donor or fill the hole of an acceptor, they create a positive or negative ion. With positive charges on one side and negative charges on the other, this essentially creates a capacitor, with an electric field pointing from the positive ions to the negative ions.), that pulls electrons back toward the N-type material and pushes holes toward the P-type material. These factors combined result in a **depletion region**, where no free movement of electrons can occur. The potential difference across the region is around 0.7V (0.3V if germanium is used instead of silicon)
#### Achieving Forward and Reverse Bias
![[Pasted image 20260809165827.png]]
When a voltage source is connected to the diode, with the negative end to the cathode and the positive end to the anode, forward bias is achieved as electrons flow from the cathode to the anode if the *voltage is greater than the 0.7V barrier.*

![[Pasted image 20260809170035.png]]
If we reverse the direction of the voltage source, where the positive end is connected to the cathode and the negative end is connected to the anode, the electrons in the N-type material are attracted toward the cathode and the 'holes' toward the anode. Current will not flow, and reverse bias is achieved.
### Technical Details
#### Identifying Diodes
![[Pasted image 20260809170401.png]]
These are the symbols for diodes in an electrical diagram. The stripe is represented as a vertical line, with an arrowhead indicating the direction of *conventional* current (electrons are flowing in the opposite direction in actuality).

![[Pasted image 20260809170553.png]]
The physical diode will have an identifying label on it indicating its max voltage and current parameters.
#### Plotting a Diode
![[Pasted image 20260809171045.png]]Above is a graph plotting the  behavior of a diode. When applied voltage is *less than 0.7V*, the diode behaves as an insulator, blocking current from flowing. However, once a certain threshold is reached (called the **max reverse voltage**), the diode, and likely the rest of the circuit, breaks, and no longer works to block current from flowing. When the applied voltage is *greater than 0.7V*, the diode behaves as a conductor, and the current allowed through increases rapidly with voltage.

The diode also provides a voltage drop of 0.7V to the circuit.
### Why use Diodes
#### Protecting Circuits
![[Pasted image 20260809171210.png]]
Diodes are used to prevent current from flowing in the opposite direction. This is useful for protecting components and devices in a circuit in the event the voltage source is installed in the wrong direction.
#### Converting AC to DC via Rectifiers
![[Pasted image 20260809171453.png]]
Because of its ability to only allow current to flow in one direction, diodes can be used to create **rectifiers** to smooth out alternating current by blocking the negative current from flowing. In this example, A transformer receives incoming AC current, and its output is connected to a single diode. The diode only allows half the wave (the positive half) to pass through. This is known as a **half wave rectifier**.

![[Pasted image 20260809173529.png|459]]
An improvement on this is the **full bridge rectifier**, which sees four diodes arranged in a closed loop (the "bridge"). This arrangement forces both halves of AC current to flow in the same direction.
##### The Positive Half-Cycle
![[Pasted image 20260809173616.png]]
During the positive half of the AC cycle, the upper AC terminal becomes positive, and the lower terminal becomes negative (conventionally).
- Diodes D<sub>1</sub> and D<sub>2</sub> become forward-biased (ON) while diodes D<sub>3</sub> and D<sub>4</sub> become reverse-biased (OFF)
- Current flows from the upper AC terminal through D<sub>1</sub>, through the load resistor from top to bottom (the DC output), through D<sub>2</sub> and to the lower AC terminal.
##### The Negative Half-Cycle
![[Pasted image 20260809173629.png]]
During the negative half of the AC cycle, the lower AC terminal becomes positive and the upper terminal becomes negative.
-  Diodes D<sub>3</sub> and D<sub>4</sub> become forward-biased (ON) while diodes D<sub>1</sub> and D<sub>2</sub> become reverse-biased (OFF)
- Current flows from the lower AC terminal, through D<sub>3</sub>, through the load resistor from top to bottom, then through D<sub>4</sub> and to the upper AC terminal.

![[Pasted image 20260809174212.png]]
The waveform above is the output of a full bridge rectifier. Both the positive and negative half-cycles of AC are forced to flow through the load in the same direction, resulting in consistent positive DC current. It is to be noted that since the current flows through two diodes at any given time, the output voltage will be 1.4V less than the input.
##### Using a Smoothing Capacitor
![[Pasted image 20260809174316.png]]
The rectifier can be improved further using a [[Capacitors||capacitor]]. The capacitor charges with excess voltage during the peaks, and discharges during the troughs to smoothen the output.
### Testing Diodes
![[Pasted image 20260809174555.png]]
To test a diode, we switch the multimeter to the diode setting, and connect the black terminal to the end of the diode with the stripe, and the red terminal to the other end. The reading given by the multimeter is the minimum voltage required for current to flow through.

![[Pasted image 20260809174858.png]]
Reversing the diode produces "OL" on the multimeter, meaning "outside limits." This means that the circuit was not completed and the multimeter could not get a reading, which means the diode is working and is blocking current flow in the opposite direction. A reading in this configuration means that the diode is faulty and should not be used.

![[Pasted image 20260809175109.png]]
To read the voltage drop across the diode in a live circuit, set the multimeter to the DC voltage setting and place the terminals at the ends of the diode (black to striped, red to other). The multimeter should give a reading indicating the voltage drop across the diode.

---
### References
![](https://www.youtube.com/watch?v=Fwj_d3uO5g8&t=302s)