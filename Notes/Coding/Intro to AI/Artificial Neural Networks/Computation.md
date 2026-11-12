Neural networks are connectionist models inspired by biological neurons, where many simple computational units work together to map inputs to outputs. **Computation** deals with what the network does once parameters have been defined and fixed, while **learning** deals with choosing those parameters based on data. Specifically, we are dealing with NN within the framework of supervised learning (classification, regression).
### The Network
ANNs adopt the idea of interconnected units from biological neurons, which receive and send signals to and from other connected neurons. Each unit in a NN combines its inputs to produce an output. When interconnected to form a network, these units collectively perform complex computation.

Each unit combines its inputs using a **weighted sum** and a **bias**, then applies an **activation function**. With several layers in the network, inputs are transformed by intermediate units before the output is computed. 

Architecture determines which units are connected; the weights and biases determine the function computed by that architecture. Training adjusts these parameters using data to improve the model's prediction performance.
### Threshold Units
The **threshold unit** is neuron idealized by McCulloch and Pitts (1943). An individual unit computes Boolean logic.

![[Pasted image 20261112183536.png]]

Two binary inputs x₁ and x₂ ∈ {0, 1} go into one unit that sums them up as $z=x_1+x_2$. Then, it applies a **step function** with threshold θ, which gives one of two outputs:
- $f(z)=0$ if $z < \theta$
- $f(z)=1$ if $z>=θ$
Hence, the unit is either 'on' or 'off' based on whether the total input reached the threshold or not.

Choosing θ gives a logic gate.
- **AND**: When θ = 2, the sum much reach two, which only happens when both inputs are 1.
- **OR**: When θ = 1, which can happen if at least one input is 1.
- **NOT**: For a single input x with weight -1 and θ = -0.5. A negative weight alone does not specify a gate; it depends on the threshold and other inputs.
AND, OR, and NOT units can be combined into circuits to compute any Boolean function.
#### Simple Step Functions
![[Pasted image 20261112185122.png|300]]![[Pasted image 20261112185139.png|282]]
- **Heaviside function**: 1 if t >= 0, 0 if t < 0
- **Sign function**: 1 if t > 0, 0 if t = 0, -1 if t < 0.
#### Bias
![[Pasted image 20261112185422.png]]
The **bias**/**intercept** is another input feature with a fixed value.
### Perceptron
![[Pasted image 20261112185642.png]]
A **perceptron** is the combination of threshold logic units arranged in a single layer. With each connected to all inputs, this configuration is known as **fully connected** or **dense**. Since the units in this layer also generate an output, it is referred to as the **output layer**.

With multiple outputs, a perceptron can perform multiple binary predictions, which makes either a **multilabel** or **multiclass** classifier.