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
Representing a func