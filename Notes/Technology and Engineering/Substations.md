Tags: #infrastructure 

![[Pasted image 20260618104030.png]]
Electricity flows through the [[Power Grids||power grid]] in three distinct steps: generation, transmission, and distribution. **Substations** serve as the linking interfaces between these steps.
### Switching
![[Pasted image 20260618104551.png]]
A substation serves as the terminal for many several incoming power lines (called **conductors**) before heading out to their next step in the power grid. The multiple power lines ensure redundancy in the event that any one line is out of commission. If any part of the system requires maintenance or repair, **switches** are in place to disconnect it; shutting off high voltages can be risky, as electricity can still travel through the air after a line breaks in the form of an **arc**.
![[Pasted image 20260618104842.png]]
Arcs are dangerous and can cause damage to equipment.
### Fault Protection
A **fault**, or **short-circuit**, is a failure that can result from any number of unpredictable outside forces. Because of the complexity of the power grid, the failure can cascade in uncontrollable ways, leaving huge populations without power. In many ways, fault protection is handled at a substation.

![[Pasted image 20260618105318.png]]
![[Pasted image 20260618105458.png|414]]
One of the most common types of faults is a short-circuit to ground. This fault creates a low-resistance path for current to flow through, which causes the current to spike and overloads the power lines and equipment. The simplest form of protection is a **fuse**, which is a subsection of the line that, when a threshold of current is surpassed, burns out and severs the line. While they are simple and don't require maintenance, they aren't reusable, and don't protect against other types of faults.

![[Pasted image 20260618105623.png]]
In a substation, a **circuit breaker** is similar to a fuse, but protects against a wider range of fault types.

![[Pasted image 20260618105823.png|270]]![[Pasted image 20260618110538.png|399]]
Inside the breaker, the circuit is kept continuous via spring-loaded electrical contacts held together by a latch. The current passes through a small electromagnet, which under normal conditions, produces a weak magnetic field. During a short circuit, where current is above safe levels, a powerful magnetic field is created, which flips a switch that releases the latch, separating the contacts and breaking the circuit. However, the electricity will still form an arc in the gap between the contacts, which can be destructive to the equipment around it. Thus, a **dielectric** (non-conductive material) is used to extinguish the arc.

Many faults are quick and temporary (lightning strikes, swaying tree branches), so some breakers have a **recloser** that can interrupt the current for a short period of time before resuming. It can do this several times to test whether the fault has cleared, before deciding on whether to resume service.

Substations can also be used to shed load if demand on the grid becomes too high, implementing rolling blackouts.
### Transformation
At power plants, electricity is often produced at low voltage. The voltage is then greatly increased when it is transmitted via [[Transmission Lines||Power Lines]] to increase its efficiency across greater distances, as higher voltages reduces power loss during travel, but it is also more dangerous, and can't be feasibly used by consumers. Thus substations use a **transformer** to reduce the voltage.

![[Pasted image 20260618111508.png]]
A transformer contains two coils of wire; one having a greater number of loops than the other. When incoming high voltage current passes through one coil, it induces a magnetic field that creates flowing current in the other wire. The ratio of coils determines the reduction of voltage from one wire to the other; if there are 1000 coils in the incoming wire and 100 coils in the outgoing wire, the resulting voltage will be reduced to a tenth. 

While the simplicity of transformers allows them to be widespread, it also makes it difficult to make adjustments once they are in place. **Instrument transformers** are used to monitor the voltage or current on a grid, or to power other monitoring devices.

![[Pasted image 20260618112236.png]]
As voltage can fluctuate due to losses associated with transmission or due to high demand, **regulators**, which are essentially specialized transformers, use a **tap changer** mechanism to maintain a stable voltage level. If a voltage drops too low or high, the regulator switches to a different "tap" to bring the voltage in the opposite direction.
### Safety
As substations are usually the only places where high-voltage transmission lines run close to the ground, it is important that safety measures are in place for electricians and linemen to perform inspections, maintenance, and repairs.
![[Pasted image 20260618112951.png]]
**Insulators** are used on bus lines to prevent short-circuit, avoiding arcs to ground.

![[Pasted image 20260618113003.png]]
At the junction between conductors and equipment, **bushings** are hollow insulators used to separate the high-voltage lines with the grounded metal casing of equipment.

![[Pasted image 20260618113344.png]]
Some substations have concrete **fire barriers** between equipment.

![[Pasted image 20260618113427.png]]
All substations have an array of grounding rods and conductors underground. In the event of a fault, the substation needs to trip the breakers as soon as possible by sinking lots of current into the ground. The grounding grid also ensures that all equipment in the station are kept at the same voltage level, thereby maintaining **equipotential**; a person touching any piece of equipment will not create an electrical path through their body.

![[Pasted image 20260618113719.png]]
Substations are also surrounded by fences and warning signs as a barrier against outsiders.

---
### References
![](https://www.youtube.com/watch?v=7Q-aVBv7PWM)