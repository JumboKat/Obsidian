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

Backpropagation is an algorithm that methodically computes the partial derivatives of the loss function with respect to each weight and bias parameter. It applies the **chain rule** recursively.
### Chain Rule
The chain rule in calculus determines the derivative of a composite function. If a function h is the composite of two functions f and g, the chain rule states:
$$h'(x)=f'(g(x))g'(x)$$
Or in Leibniz notation:
$$\frac{dv}{dx}=\frac{dv}{du}*\frac{du}{dx},\ u=g(x),\ v=f(x)$$
If you know the instantaneous ROC of v w.r.t. u, and the ROC of u w.r.t. to x, then the ROC of v w.r.t. x is the product of these two ROCs. 

#### Applying the Chain Rule Recursively
![[Pasted image 20261114180200.png|441]]
For a downstream route: 
$$\frac{dl}{du}=\frac{dz}{du}*\frac{dl}{dz}$$ Backpropagation decomposes the computation into a series of small steps, applying chain rule at each one. The pink box computes an output $z$ from its inputs $v$ and $u$. The loss for one example is denoted by $l$. The partial derivative $\frac{dl}{du}$ measures the loss's local sensitivity to $u$. The sign indicates the effect of a small increase in $u$, and its magnitude measures the sensitivity. 
#### Example
![[Pasted image 20261114182017.png|429]]