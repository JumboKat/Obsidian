Tags: #infrastructure
### Intersections
**Arterial roadways** are high-capacity urban roads that serve as the backbone of city transit grids. They move large volumes of traffic over long distances, connecting smaller collector roads via controlled intersections. They use at-grade (level with the natural ground) intersections to limit passing traffic to just a few streams at a time, which we call "interrupted traffic flow." 

Intersections are often the primary bottleneck for the roadway's throughput, meaning increasing the number of lanes or the speed limit will not impact overall traffic efficiency. Additionally, intersections are where most accidents occur, and so much consideration is put into making them safe and efficient.

Traffic lights are a common and popular solution to assigning right-of-way. While while road signs are simpler and not as costly, they don't handle high traffic volumes well as they require each vehicle to make an individual stop. The basic universal traffic light has three lights: green for go, red for stop, and amber indicating that the light is about to change from green to red.
### Phases of an Intersection
At each intersection, vehicles can make a movement in three directions: right, through, or left. Since we drive on the right side of the road, right and through are grouped together as a single movement. A typical four-way intersection has 8 vehicle and 4 pedestrian movements. Non-conflicting movements can be grouped into phases of the traffic signal (ex. the left turn of opposite approaches can be grouped into one phase, as they do not obstruct each other).

![[Pasted image 20260706163419.png]]
Traffic engineers sketch out a **ring-and-barrier** diagram to plan out how the traffic signal phases will operate. Above is an example signal cycle:
1. Major street left turns.
2. Major street and pedestrian through movements.
3. A "barrier" to clear the intersection
4. Minor street left turns.
5. Minor street and pedestrian through movements.
6. A barrier marking the beginning of a new cycle.

Many factors and decisions must be made when designing the traffic signal phases of any given intersection. One important decision is the timing of phases. Ideally, the green light should be long enough to at least clear the queue of vehicles built up during the red light. During peak times, this can be extended to minimize start up and clearance times. The amber light must be long enough to be perceived by drivers and allow them to comfortably deaccelerate their vehicles. In most regions of North America, you are allowed to enter an intersection at any point of a yellow light, meaning that there must be enough time where all phases have a red light for the intersection to clear.
### Actuated Signal Control
![[Pasted image 20260706214351.png]]
Most traffic signal systems are more complex than just signals on a set timing sequence. Systems that use **actuated signal control** receive input signals from traffic detection systems to make decisions on signal timing and sequences in real time. Some of these detectors take the form of cameras or radars, but the most common method of detection involves inductive loop sensors embedded in the surface of the road, essentially working like large metal detectors.

![[Pasted image 20260706215152.png]]
The data is then sent to an equipment cabinet nearby, which contains a computer that calculates traffic signal timing and sequencing based of the traffic data. This way, traffic signals do not need to be manually adjusted for special events or road closures. They can also give priority to emergency vehicles by using special detectors like infrared or acoustic sensors to communicated with specific types of vehicles.
### Signal Coordination
Intersections are not isolated systems, and traffic control signals must coordinate with each other to increase efficiency and avoid bottlenecks.

![[Pasted image 20260706215640.png]]
As an example, if two intersections are right next to each other, one may signal cars to go while the next is at a red light, which can cause a backup of cars spanning multiple cycles at multiple intersections. Signals therefore take into account the traffic of adjacent intersections when deciding their timing and sequencing. The signals on the main road are ideally timed so that a large grouping of vehicles, called a **platoon**, are able to pass through multiple intersections without stopping. This configuration only really works when there are no other traffic interruptions, like driveways and roadside businesses. If the platoon does not stick together, the benefits of coordination are forfeit.

![[Pasted image 20260706220144.png]]
The overall solution is the coordination of all traffic signals within a traffic network, a job carried out by **adaptive signal control technologies (ASCT)**. All the information is fed to a centralized decision-making system that makes use of advanced algorithms like machine learning.

---
### References
![](https://www.youtube.com/watch?v=DP62ogEZgkI&t=302s)