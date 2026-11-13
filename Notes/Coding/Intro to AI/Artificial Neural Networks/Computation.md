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

With multiple outputs, a perceptron can perform multiple binary predictions, which makes either a **multilabel** (each instance can fit multiple labels simultaneously) or **multiclass** (each instance assigned to one class out of many mutually exclusive options) classifier.
#### Notation
- **X** is the input data matrix where each row corresponds to an example and each column represents one of the **D** features.
- **W** is the weight matrix, with one row per input feature and one column per neuron. **w** is a weight vector.
- Bias terms can be included above or represented separately. Separately, it is a vector with length = # of neurons.
### XOR
![[Pasted image 20261112190739.png]]
 For binary inputs, XOR is 1 at (0,1) and (1,0) and 0 at (0,0) and (1,1). XOR is not linearly separable in the original input space; a single threshold unit cannot separate the two XOR classes with one straight line. XOR must be implemented via a **multilayer perceptron (MLP)**:
 ![[Pasted image 20261112191031.png]]
An MLP has one or more **hidden layers** between its inputs and output layer. The hidden units in this example compute AND (top) and OR (bottom). Altogether, XOR is computed as:
$$A⊕B=(A∨B)∧¬(A∧B)$$
or A XOR B = (A OR B) AND NOT (A AND B).

![[Pasted image 20261113125906.png]]The output computes A AND NOT B. The hidden representation makes XOR linearly separable.

**The credit assignment problem**: generally, hidden-unit targets aren't given, so training must figure out how hidden weights affect the final error. This can be solved by **backpropagation**. 
### Feedforward Neural Network (FNN)
![[Pasted image 20261113130850.png|479]]
In this architecture, information flows forward only, from input to output. The diagram above shows a **two-layer network**, as it has only two layers that perform parameterized computations (hidden layer and output layer). **Input units** or **input neurons** simply introduce input features without performing any calculations on them. There is typically as many input units as there are features.

Nodes are labelled to clarify the weights between them. For instance, $w_{1,5}$ is between nodes 1 and 5. The output of a node is written as $o_k$, with k being the node's label. 
#### Weighted Sum and Activation
Each unit performs two operations:
- $z_j=\sum_{i→j}w_{i,j}o_i+b_j$ , the weighted sum + bias before activation
- $o_j=φ(z_j)$, the output after applying activation φ.

The sum runs over all nodes connec
ted to unit j. The weight $w_{i,j}$ connects source node i to destination node j, and $b_j$ is the bias of unit i. 

$z_j$ is also called the **preactivation**, while output $o_j$ is the **activation**. φ represents a general activation function, though σ is used when the activation is the logistic sigmoid.
#### Forward Pass Computation
![[Pasted image 20261113130850.png|408]]
The computation for this FNN is as follows:

Calculate hidden outputs first:
$$o_3=\sigma(w_{1,3}x^{(1)}+(w_{2,3}x^{(2)}+b_3)$$
$$o_4=\sigma(w_{1,4}x^{(1)}+(w_{2,4}x^{(2)}+b_4)$$
$$o_5=\sigma(w_{1,5}x^{(1)}+(w_{2,5}x^{(2)}+b_5)$$
For final outputs:
$$o_6=\sigma(w_{3,6}o_3+w_{4,6}o_4+w_{5,6}o_5+b_6)$$
$$o_7=\sigma(w_{3,7}o_3+w_{4,7}o_4+w_{5,7}o_5+b_7)$$
Each computational unit has its own bias, independent of its inputs. For a threshold unit, it shifts the decision boundary away from the origin. For a sigmoid or ReLU unit, it shifts the position of the activation's transition.
### Nonlinear Activation Functions
Stacking layers with without a nonlinear activation (the weighted sum is always linear) only results in one **affine** linear transformation. Nonlinear activations allow the network to represent more complex input-output relationships. 

An affine transformation is a weighted sum + bias. For a 1-D example, composing $h=w_1x+b_y$ with $y=w_2h+b_2$ gives:
$$y=(w_1w_2)x+(w_2b_1+b_2)$$
Which is another affine transformation. This applies to matrix computations as well, so stacking affine layers alone does not allow the network to represent more complex relationships like XOR.

Activation functions play two roles: nonlinearity affects what can be represented, and derivatives affect trainability.
#### Sigmoid
![[Pasted image 20261113134649.png]]
The sigmoid function maps any finite real input to a value between 0 and 1 using the formula:
$$\sigma(z)=\frac{1}{1+e^{-z}}$$
#### Hyperbolic Tangent (tanh)
![[Pasted image 20261113134854.png|569]]
Unlike sigmoid, tanh can produce negative and positive activations (-1, 1), with tanh(0) = 0. Its output range is centered on zero, but the actual mean activation still depends on inputs and parameters. Its formula is: $$tanh(z)=2\sigma(2z)-1$$
#### Rectified Linear Unit (ReLU)
![[Pasted image 20261113135044.png]]
Common choice for hidden layers. It returns zero for negative inputs, while keeping positive inputs unchanged. The change of slope at zero makes it nonlinear. Composing affine transformations with ReLU can produce piecewise-linear functions with many regions. Its formula is:$$ReLU(z)=max(0,z)$$
#### Comparing Activation Functions
![[Pasted image 20261113135251.png]]
### Universal Approximation
The **universal approximation theorem (UAT)** states that on a compact input domain (closed and bounded), a network with a sufficiently wide single hidden layer, sigmoid activations, and a linear output can approximate any specified continuous target function to any desired accuracy.

In other words, as long as network can be made wide enough, one hidden layer is enough to mimic any continuous function as closely as you like, for a bounded region of inputs. This only describes what a network can represent, it does not guarantee:
- that a training algorithm will find the right parameters.
- that the network will be small.
- that learning from a finite sample will generalize well.
- good extrapolation outside the domain.

It extends to other activations like ReLU, but the activation must be nonpolynomial; nonlinearity alone isn't sufficient.

![[Pasted image 20261113162327.png]]
The graph above shows how increasing hidden units (which increases the number of bends) can help closer represent the target function. The UAT guarantees the existence of an approximation under its assumptions, not the success of its training runs.
### Keras
**PyTorch**, **TensorFlow**, and **JAX** provide tools for numerical computation and automatic differentiation. **Keras** provides a high-level API for building and training models with these backends.

A dense layer with n inputs and m outputs has (n + 1)m trainable parameters, including biases. 