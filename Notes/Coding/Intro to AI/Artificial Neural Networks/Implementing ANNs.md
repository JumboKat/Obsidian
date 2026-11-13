The make_circles generated dataset will be used for this demonstration. It has an outer ring (class 0) and an inner ring (class 1). These classes are not linearly separable in the original (x1, x2) feature space.
![[Pasted image 20261113164216.png|388]]

![[Pasted image 20261113164420.png|449]]
The architecture implemented is a multi-layer perceptron consisting of one input layer, two hidden layers with four units each, and one output layer with a single unit. This architecture is denoted as \[2, 4, 4, 1]. The network contains three parameterized layers (two hidden + 1 output). Every computation unit uses the sigmoid activation function. The final output estimates the probability of class 1. Each also has a bias term equal to 1.
### Parameters
![[Pasted image 20261113165050.png]]
Each layer takes in `in_dim` numbers and produces `out_dim` numbers. Its weight matrix has the shape W - (in_dim, out_dim), and its bias has shape (out_dim).

The number of rows = number of inputs, while number of columns = units in the layer. So each column of W holds the weights for one neuron, and each row holds the weights leaving one input.

In our example, ℓ(1) (hidden 1) has two inputs and four outputs, so its weight matrix is 2 x 4 with its bias vector being four wide. ℓ(2) has four inputs and four outputs, so its W is 4x4. ℓ(out), or the output layer, has four inputs and 1 output, so its W is 4x1 with a bias of width 1. The total weights for this NN is 8+16+4=28, with 4+4+1 bias terms. Combined, this makes 37 parameters for this network.

Matrix multiplication requires the number of features to match the number of inputs to the layer (columns in X matches rows in W)

![[Pasted image 20261113170944.png]]
For neuron 1 in layer $ℓ_1$, the pre-activation value is computed as:$$z_1=x_1W_{1,1}+x_2W{1,2}+b_1$$
#### NumPy Broadcasting
![[Pasted image 20261113171339.png|431]]
The big empty grid is the matrix A = X @ W($ℓ_1$). The bias vector only has four entries. Normally, these do not line up for matrix multiplication. NumPy's broadcasting rule matches the four entries to the four columns, then stretches b down the rows, essentially copying them into a (10, 4) matrix. $b_1$ is added to every entry in column 1, $b_2$ is added to every entry in column 2, etc. Each column corresponds to one neuron, and a neuron has one bias shared across all examples. 
### Predictions
The `forward` method creates an array of probabilities ranging from 0 to 1, representing the likelihood of that particular example $x_i$ belonging to class 1. Evaluating `self.forward(X)>=threshold` gives an array of Boolean values; True if the probability is above the threshold, and False otherwise. These are then converted into binary 0s and 1s.