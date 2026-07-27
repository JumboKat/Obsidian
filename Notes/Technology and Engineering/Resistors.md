![[Pasted image 20260724110331.png]]
![[Pasted image 20260724110347.png]]
Resistors come in many forms, and are represented by many different symbols in circuit diagrams to reflect this. The purpose of a resistor is to convert excess current in a circuit into heat to protect electronics from being overloaded and damaged/destroyed.
### Resistance
![[Pasted image 20260724110726.png]]
A resistor provides resistance to the flow of electrons (measured in Ω/ohms). It does not slow the speed of electrons, but rather limits how many can pass through at any given time. The resistor contains obstacles that electrons collide with, producing heat. This also results in a voltage drop across the resistor.
### Types of Resistors
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
#### Potentiometer
