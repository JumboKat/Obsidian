Neural networks are connectionist models inspired by biological neurons, where many simple computational units work together to map inputs to outputs. **Computation** deals with what the network does once parameters have been defined and fixed, while **learning** deals with choosing those parameters based on data. Specifically, we are dealing with NN within the framework of supervised learning (classification, regression).
### The Network
ANNs adopt the idea of interconnected units from biological neurons, which receive and send signals to and from other connected neurons. Each unit in a NN combines its inputs to produce an output. When interconnected to form a network, these units collectively perform complex computation.

Each unit combines its inputs using a **weighted sum** and a **bias**, then applies an **activation function**. With several layers in the network, inputs are transformed by intermediate units before the output is computed. 

Architecture determines which units are connected; the weights and biases determine the function computed by that architecture. Training adjusts these parameters using data to improve the model's prediction performance.
### Threshold Units
The **threshold unit** is neuron idealized by McCulloch and Pitts (1943). An individual unit computes Boolean logic.

![[Pasted image 20261112183536.png]]

Two binary inputs x₁ and x₂ ∈ {0, 1} go into one unit