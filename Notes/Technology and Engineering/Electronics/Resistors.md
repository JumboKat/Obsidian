![[Pasted image 20260724110331.png]]
![[Pasted image 20260724110347.png]]
Resistors come in many forms, and are represented by many different symbols in circuit diagrams to reflect this. The purpose of a resistor is to convert excess current in a circuit into heat to protect electronics from being overloaded and damaged/destroyed.
### Resistance
![[Pasted image 20260724110726.png]]
A resistor provides resistance to the flow of electrons (measured in Ω/ohms). It does not slow the speed of electrons, but rather limits how many can pass through at any given time. The resistor contains obstacles that electrons collide with, producing heat. This also results in a voltage drop across the resistor.
### Types of Resistors - Invariable Resistors
#### Carbon Composite Resistor
![[Pasted image 20260724111419.png]]
The core of a carbon composite resistor is made by mixing a conducting material (graphite) with an insulating material (ceramic). Metal connectors are attached to each end, with the core wrapped in an insulating case. Electrons flow through the solid core. This type of resistor has fallen out of use, as modern versions are more efficient and robust.
#### Carbon Film Resistor
![[Pasted image 20260724112652.png]]
The typical resistor used in circuit boards. In a carbon film resistor, a ceramic core is wrapped in a carbon film. The metal connectors are attached with end caps, and everything is covered by an insulating case.

![[Pasted image 20260724112721.png]]
A helical groove is cut into the carbon layer, creating a narrow path of high resistance for electrons to travel through.

![[Pasted image 20260724112912.png]]
We can alter the level of resistance by changing the width of the path and the number of rotations. The larger a resistor, the more heat it can dissipate due to the larger surface area. Hence, larger resistors will have greater power ratings.
##### Reading 4-Band Resistors
![[Pasted image 20260724113155.png|288]]![[Pasted image 20260724113341.png|404]]
You can find the resistance by reading the packaging or by using a multimeter. We can also read the resistance form the resistor directly from its four stripes. The first two bands represent digits, the third represents a multiplier, and the fourth is for tolerance, which is visually separated from the others. In the example above, The first brown stripe indicates that the first digit is 1, the black indicates the second digit is 0, the third band indicates a multiplier of 10, and the gold band indicates a tolerance of ±5%. This means that the resistance is 10x10 = 100Ω ±5%, meaning it can be anywhere in the range of 95 - 105Ω.
#### Metal Film Resistor
![[Pasted image 20260724153540.png]]
Also very commonly used, a metal film resistor consists of a ceramic core coated in a thin metal layer. The metal connectors are attached via end caps. Like a carbon film resistor, a helical cut is used to control the resistance. A protective coating covers the resistor, with five coloured bands to indicate resistance level.
##### Reading 5-Band Resistors
![[Pasted image 20260724165110.png]]
Reading the resistance of a 5-band resistor is similar to reading a 4-band resistor, except that the first three bands represent digits, rather than the first two.
#### Wirewound Resistors
![[Pasted image 20260726142454.png]]
Wirewound resistors are high-power rated and come in many forms, all which share a very basic design; a metal wire wrapped around a ceramic core and covered in a thin insulating layer. The thickness, length, and material used determine the resistance. Some are buried within a block of concrete and are used for high-heat use cases. In another design, the wire is encased in aluminum casing with a ridged surface designed to dissipate heat.
#### Surface Mount Device (SMD) Resistors
![[Pasted image 20260726142842.png]]
SMD resistors usually come in very small sizes, with some even being microscopic. They consist of a ceramic core, an electrode at each end connected by a thin resistive material. This is then covered by an insulting case and capped off with metal connectors.

![[Pasted image 20260726143011.png]]
A laser-cut groove is made in the resistive material to reduce the area in which electrons can flow to increase resistance. 

While they can have high tolerances, their small size limits their ability to dissipate heat, and so have very low power ratings. On top of the case is a numeric value representing resistance.
##### Reading SMD Resistances
![[Pasted image 20260726143217.png]]In three digit SMDs, the first two represent significant values, while the third is the multiplier. For example, "101" indicates a significant value of 10 and a multiplier of 10, meaning it has a resistance of 100Ω. The same is true of four-digit SMDs, except that the first three digits are significant values, and the fourth represents the multiplier.

![[Pasted image 20260726143514.png]]
If the letter R is present, it represents a decimal value. For example, "47R5" indicates a resistance of 47.5Ω.

![[Pasted image 20260726143655.png]]
With ratings that include two digits and a letter (e.g. C or Z), we use this specific table to calculate the resistance, where the first two digits serving as the code pointing to a value, and the letter indicating the multiplier.
#### Fusible Resistors
![[Pasted image 20260727212422.png]]
A fusible resistor looks almost identical to a standard invariable resistor, but it also acts as a fuse. When a standard resistor is overloaded, it bursts into flames, but when a fusible resistor is overloaded, it shorts the circuit. It consists of a ceramic core with a resistive wire wound around it, connecting the two end caps, and covered by a protective casing. The wire acts as a fuse, breaking if a certain heat threshold is passed. Some variations have a sheet of metal rather than a wire, with a groove cut into it to increase resistance.
##### Reading Fusible Resistors
A fusible resistor has five bands, with a white band at the end identifying it as a fusible resistor. The other four indicate the resistance, and would be read as a standard four-band resistor (first two are significant values, third is the multiplier, fourth is the tolerance).
### Types of Resistors - Variable Resistors
#### Potentiometer
![[Pasted image 20260726202448.png|449]]![[Pasted image 20260726202402.png|240]]
Potentiometers contain a dial that allows for its resistance to be changed. General-use potentiometers (brown) are often used on larger-scale electronics; for instance, serving the role of volume control on a radio. Precision potentiometers (blue) are used for tuning electronics on circuit boards.

![[Pasted image 20260726202923.png]]A potentiometer has three terminals, with the two end pins connected by a circular resistive track. A dial rotates around the centre pin and connects to the track. Moving the dial increases the length electrons must travel, thus increasing the resistance. Only the centre terminal's voltage can be adjusted.
##### Reading Potentiometers
![[Pasted image 20260726203228.png]]
On general-use potentiometeres, the front will have a number and a letter. The number directly indicates the resistance in Ohms, while the letter indicates whether the resistance scales linearly with rotation (**B**) or logarithmically (**A**).

Smaller, precision potentiometers will have three digits, which are read in the same way as a three-digit SMD resistor (first two digits are values, third is the multiplier, same reference table).
#### Rheostat
![[Pasted image 20260726203944.png]]A rheostat is a variable resistor used to control the current in a circuit. They are typically very large as current tends to be high. They can have up to three or four terminals, though only two are used at a time. Smaller circuits can use a potentiometer as a rheostat. To control current, a rheostat uses a resistive wire coiled around an insulating ceramic core, which is either arced (left) or cylindrical (right). In either form, the rheostat features an arm that moves along the wire that determines how far electrons have to travel through the resistive wire, thus modifying the resistance. Maximum ratings for resistance, power, and/or current can be found marked on the side of the component.
#### Varistors
![[Pasted image 20260727212948.png]]
A **varistor** is a variable resistor similar in appearance to a ceramic capacitor, but cannot be manually controlled like a potentiometer. Instead, it changes its resistance on its own, depending on the voltage it is exposed to.

![[Pasted image 20260727213057.png]]
It is typically connected in parallel to the supply in delicate circuits. Normally, it provides very high resistance, acting like an insulator and allowing almost no current to pass through. However, after a certain voltage threshold, it behaves like a conductor and short to ground, thus protecting the circuit against spikes in voltage.

![[Pasted image 20260727213345.png]]
Inside the component lies a mixture of zinc oxide and ceramic at its core, sandwiched between two metal electrodes with metal connectors attached, and is encased in an epoxy protective casing.
##### Reading Varistors
![[Pasted image 20260727213443.png]]
On its face is a rating indicating its diameter (mm), shape (D = disk, S = square), voltage rating (value, number of zeros afterward, in this example 121 means 12 x 10 = 120V), and a letter indicating tolerance (K = ±10%)
#### Thermistors
Thermistors are variable resistors whose resistances change based on their temperature.