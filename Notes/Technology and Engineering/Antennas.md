#Technology 
### Signal Transmission
Antennas must send and receive electromagnetic waves to transmit information. An electromagnetic field can be produced using a closed conductor via **electromagnetic induction**, however the the field does not propagate, but rather simply fluctuate around its source; it is of no use for communications. An antenna must produce waves that are separate from the source and are able to propagate.
#### Wave Separation
![[Pasted image 20260714143651.png]]
To explain the physics behind wave separation, take a dipole of a positive and negative charge placed apart from each other, producing an electric field. If each oscillates up and down opposite to each other, they will reach maximum velocity at the midpoint of their paths and zero velocity at the ends of their paths. Thus, they continuously accelerate and decelerate along their paths.

![[Pasted image 20260714144116.png|346]]![[Pasted image 20260714144306.png|345]]
As the charges oscillate, they affect the shape of the magnetic field. The magnetic field does not easily adjust to the new positions of the charges, and thus experiences a memory effect, producing a kink in the field. As the charges move toward the midpoint, the waveform continues to stretch in the same manner, until the ends of the wavefront meet at a single point. As the charges move past each other, the wavefront separates and begins to propagate.

![[Pasted image 20260714144649.png|361]]![[Pasted image 20260714144802.png|325]]
If you draw field intensity with the distance, the wave propagation appears sinusoidal. The wavelength of the propagation is exactly double that of the length of the dipole. Thus, we can emit an electromagnetic wave signal as long as we use a mechanism for oscillating charges.
### Producing Oscillating Charges
#### Antenna as a Transmitter
![[Pasted image 20260714145119.png]]
We can take a conducting rod with a bend at its centre, where a time-varying voltage is applied. While the voltage applied is positive, the electrons will all congregate to one end of the rod, leaving the other side positively-charged and producing a dipole. When the voltage is reversed, the charges will move in opposite directions, giving the same charge oscillation demonstrated previously and resulting in wave propagation. This is how an antenna functions as a **transmitter**. The frequency of the wave propagation is the same as the frequency of the applied voltage. For perfect transmission, the length of the antenna should be half the wavelength.
#### Antenna as a Receiver
![[Pasted image 20260714145704.png]]
The operation of the antenna can be reversed to function as a **receiver**. When an electric field is applied, it causes the electrons to shift to one end of the rod. As the field varies, the electrons shift from one end to the other. This varying flow of electrons produces a varying voltage signal at the centre of the antenna. The frequency of the output voltage signal is the same as the frequency of the electromagnetic wave received. As with transmission, the antenna should be half the length of the wavelength it receives.
### Practical Antenna Designs
#### Yagi-Uda Antenna
**![[Pasted image 20260714150112.png]]**
In the past, antennas were used to receive television signals. The central bar serves as the dipole, while **reflectors** and **directors** help focus the signal to the dipole. The dipole antenna receives the EM wave signal and converts it into electrical signals, which were carried by coaxial cables to the television.
#### Dish TV Antenna
![[Pasted image 20260714150406.png]]
TV antennas nowadays commonly use a dish, called a **parabolic reflector**. This reflector receives wave signals from a satellite and focuses them onto a **Low-Noise Block Downconverter (LNBF)**.

![[Pasted image 20260714150622.png]]
Within an LNBF, the **feedhorn** receives signals and funnels them into the **waveguide**, which focuses them onto the **probe**. Voltage is induced at the probe as a result of electric dipoles, and is then fed to a **PCB** for signal processing, including filtration, high-to-low frequency conversion, and amplification. The signals are then carried by a coaxial cable to the TV.

![[Pasted image 20260714151003.png]]
Most LNBFs have two perpendicular probes instead of one. One detects vertically-polarized signals, while the other detects horizontally-polarized signals. Satellite provides transmit two perpendicular wave orientations at the same time to squeeze twice as many channels out of the same frequency band. Since the waves oscillate orthogonally, they do not disrupt each other.
#### Patch Antenna
![[Pasted image 20260714151350.png]]
Cellphones use **patch antennas**, which consists of a strip of metal on a ground plane with a piece of dielectric material between them. The metallic patch acts as a radiating element. The length of the patch should be half the wavelength.

---
### References
![](https://www.youtube.com/watch?v=ZaXm6wau-jc&list=PLsw0pFpRQcwAAWA42QsDcgtU4M5fUeG13&index=26)