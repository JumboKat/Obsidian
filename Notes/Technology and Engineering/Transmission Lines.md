Power plants that produce electricity are typically located far away from their end consumers. This is because rural, unpopulated land is relatively cheap, and most plants require large cooling ponds. They also aren't desirable to live nearby, as they can be a major source of air, sound, and visual pollution. Thus, most electricity is distributed long distances via electric transmission lines, or **conductors**, suspended high in the air over rural area. Because electric utilities providers only profit off of the energy that reaches consumers, it is in their interest that the transmission of electricity be as efficient as possible. 
### Energy Loss Reduction: High Voltages
When electricity travels over any distance through a medium, energy is lost as heat due to the resistance of the medium. Even good conductors like aluminum and copper offer some resistance to the flow of current.

Power is a product of current and voltage, in watts: $$P=IV$$ Ohm's Law dictates that the voltage drop from one end of the power line to the other is a product of its current and resistance, in volts: $$V = IR$$
Substituting this into the first equation gives: $$P=I^2R$$
This means that to reduce power loss in a power line, we can reduce its resistance (reduce its cross-sectional area and use a less resistive material), however reducing the current will yield a much greater reduction in power loss; cutting the current in half will cut the power loss to 1/4. Because voltage and current have an inverse relationship according to Ohm's law, we can reduce the current by increasing the voltage. Before it is sent out over transmission lines, power plants use **transformers** to boost the voltage, up to 100,000 volts or higher to reduce the current and energy loss.
### Safety Measures Against High Voltages
High voltages, while increasing transmission efficiency, also makes it more dangerous. As electricity has a natural tendency to flow from areas of high voltage to areas of low voltage, high voltages means there is more force compelling the electricity to find a path to flow through, causing it to even travel through non-conductive materials like the air, a phenomenon known as an **arc**. Thus, it is important that measures to prevent arcing and other dangers associated with high voltages are taken.

![[Pasted image 20260625151508.png]]
Most long distance power lines don't insulate the conductors themselves, as the thickness of the insulation around the wires would make the endeavor too expensive. Instead, everything is spaced far apart from each other, allowing the gaps of air to act as an insulator. Transmission towers and pylons raise the conductors high above the ground to prevent anything on the ground from getting close enough to create an arc.
#### Three-phase Alternating Current System
![[Pasted image 20260625151734.png]]
Most transmission towers run wires grouped in multiples of three. This is because the grid uses a [[three-phase alternating current (AC) system]], sending alternating current through each wire simultaneously, with the current in each offset from each other by $120^\circ$. In a single-phase system, the voltage will drop to zero twice every cycle. In a three-phase system, as each is phase shifted by one-third of a full cycle, one of the wires is always transmitting at or near peak power. As one drops in voltage, the second is rising, with the third at its minimum trough. 

![[Pasted image 20260625152430.png]]
These three wires are spaced apart from each other to prevent arcing. The wires also connect to each tower via long insulators, maintaining a sufficient distance between the lines and the grounded pylons.

![[Pasted image 20260625152540.png]]
The insulators are usually comprised of several ceramic discs so that if they get wet, electricity leakage is forced to take a longer path to ground. The discs are standardized in such a way that the voltage (in kilovolts) can be identified by simply multiplying the number of discs by 15.

A smaller conductor, called a **shield** or **static** wire can also be seen running above the actual power lines. They do not carry any current. Instead, they protect the actual conductors from lightning strikes.
### Other Design Factors
Outside of high voltages, there are many factors that can impact the efficiency and safety of power lines. The choice of the conductor material itself is a balancing act of robustness and energy loss. Transmission lines are so long that even a minor change in size or material can drastically alter the overall cost. 

Conductors are rated by how much current they can carry for a given rise in temperature. At peak demand, they can get hot and sag, which can create a fire hazard if tree branches are too close. 

![[Pasted image 20260625153543.png]]
Wind can also create harsh oscillations that can cause damage or failure. **Stockbridge dampers** can be used to absorb some of the wind's energy.

High voltages can also create magnetic fields around the conductors, which can induce current in parallel conductors like fences and interfere with magnetic devices, so the height of the towers are sometimes set based on the size of the electro-magnetic field. Minimizing noise, which can be disruptive to nearby residents, is also a factor to consider.

---
### References
![](https://www.youtube.com/watch?v=qjY31x0m3d8&list=PLsw0pFpRQcwAAWA42QsDcgtU4M5fUeG13&index=30)