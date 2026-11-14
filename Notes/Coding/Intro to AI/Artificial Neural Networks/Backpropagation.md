### Notation
- $z^{(l)}$ - preactivations: weighted sums + biases in layer $l$
- $a^{(l)}$ - activations: the layer's outputs
- φ -  a general activation function, applied elementwise
- $\sigma$ - the logistic sigmoid activation function
- $W^{(l)},b^{(l)}$ - the weights and biases of layer $l$
- $f_1$ - the complete layer transformation: affine computation followed by activation.

Subscripts select a feature, while superscripts $^{(l)}$ select a layer.

Lowercase a and z describe one example, while uppercase A and Z describe a batch of examples stored as rows.
### Backpropagation
Representing a function and learning suitable parameters are distinct problems. For a new task, training must determine how each hidden-layer parameter influences the loss even though target values for hidden units are not supplied. Backpropagation efficiently computes those sensitivities. 

A **forward pass** computes activations; a **backward pass** computes parameter gradients. **Gradient descent** uses those gradients to update parameters. A positive derivative means decrease the parameter, while a negative derivative means increase it. 

Backpropagation is an algorithm that methodically computes the partial derivatives of the loss function with respect to each weight and bias parameter. It applies the **chain rule** recursively to compute the 