The make_circles generated dataset will be used for this demonstration. It has an outer ring (class 0) and an inner ring (class 1). These classes are not linearly separable in the original (x1, x2) feature space.
![[Pasted image 20261113164216.png|388]]

![[Pasted image 20261113164420.png|449]]
The architecture implemented is a multi-layer perceptron consisting of one input layer, two hidden layers with four units each, and one output layer with a single unit. This architecture is denoted as \[2, 4, 4, 1]. The network contains three parameterized layers (two hidden + 1 output). Every computation unit uses the sigmoid activation function. The final output estimates the probability of class 1. Each also has a bias term equal to 1.

![[Pasted image 20261113165050.png]]
Each layer takes in `in_dim` numbers and produces `out_dim` numbers. Its weight matrix has the shape W - (in_dim, out_dim), and its bias has shape (out_dim).

The number of rows = number of inputs, while number of columns = units in the layer. So each column of W holds the weights for one neuron, and each row holds the weights leaving one input.

In our example, ℓ(1) (hidden 1) has two inputs 