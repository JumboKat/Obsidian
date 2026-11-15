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
#### Example: Scalar Input, One Hidden Node
![[Pasted image 20261114182456.png|518]]![[Pasted image 20261114182515.png|388]]

This is an example with a scalar input and one unit per layer. Each processing layer has one weight, one bias, one preactivation, and one activation, written as $w_l, b_l, z_l, a_l$. Thus, $a_l$ is the single entry of $a^{(l)}$ and $z_l$ is the single entry of $z^{(l)}$. 

##### Step 1: Forward Pass
The second diagram separates each layer into its preactivation and activation. At the first level, the preactivation $z_1$ is equal to the weight $w_1$ times x + $b_1$. The activation is just the sigmoid applied to $z_1$. The preactivation for layer 2 is the layer's weight times the activation of layer 1 + layer 2's bias. Its activation, which is also the NN's output, is the sigmoid applied to layer 2's preactivation:
$$
\begin{aligned}
z_1&=w_1*x+b_1 \\
a_1&=\sigma(z1) \\
z_2&=w_2*a_1+b_2 \\
a_2&=\sigma(z_2) \\
\hat{p}&=a_2
\end{aligned}$$
##### Step 2: Compute Local Derivatives
Now, we must find each local derivative:
- Loss (w.r.t. $\hat{p}$): $\frac{dl}{d\hat{p}}=-(\frac{y}{\hat{p}}-\frac{1-y}{1-\hat{p}})$ 
- $\hat{p}$ derivative w.r.t. $z_2$: $\frac{d\hat{p}}{dz_2}= \sigma'(z_2)=\hat{p}(1-\hat{p})$
- Local derivatives of $z_2$ = $w_2a_1+b_2$: $\frac{dz_2}{dw_2}=a_1,\ \frac{dz_2}{db_2}=1, \ \frac{dz_2}{da_1}=w_2$
- $a_1=\sigma(z_1)$: $\frac{da_1}{dz_1}=\sigma'(z_1)=a_1(1-a_1)$
- $z_1 = w_1x+b_1$: $\frac{dz_1}{dw_1}=x,\ \frac{dz_1}{db_1}=1\ \frac{dz_1}{dx}=w_1$
##### Step 3: Chain them for Each Parameter
Multiply along the path from the loss back to the parameter.
For loss w.r.t. $w_2$: $\frac{dl}{dw_2}=\frac{dl}{d\hat{p}}*\frac{d\hat{p}}{dz_2}*\frac{dz_2}{dw_2}=(\hat{p}-y)a_1$
![[Pasted image 20261114185354.png]]

For loss w.r.t. $b_2$:
![[Pasted image 20261114185730.png]]

For loss w.r.t. $w_1$
![[Pasted image 20261114185805.png]]
![[Pasted image 20261114185814.png]]

And loss w.r.t. $b_2$:
![[Pasted image 20261114185834.png]]

In the end we get:
![[Pasted image 20261114185906.png]]
![[Pasted image 20261114190253.png]]
##### Step 4: Error Signals
Some derivative terms are used multiple times ($\frac{dl}{d\hat{p}}*\frac{d\hat{p}}{dz_2}=\hat{p}-y$). Recomputing them results in a lot of waste. Save them as **error signals:**
- $\delta_2=\frac{dl}{d\hat{p}}*\frac{d\hat{p}}{dz_2}=\hat{p}-y$  
- $\delta_1=\delta_2 \cdot \frac{dz_2}{da_1} \cdot \frac{a_1}{dz_1} =\delta_2\cdot w_2\cdot a_1(1-a_1)$ 
Hence, we can rewrite each partial derivative/gradients as:
![[Pasted image 20261114190930.png]]

Which is the same as:
![[Pasted image 20261114192408.png]]

$\delta_2$ is the sensitivity of the loss to $z_2$ (output layer), and δₗ = ∂ℓ/∂zₗ is how sensitive the loss is to changes in $z_1$ (hidden layer). These are error signals, and the index matches the layer and is tied to z, since every parameter in each layer enters the loss through z. If you know how loss responds to a layer's z, you're one cheap step away from the gradient of any single weight or bias in that layer.

Additionally, a hidden unit does not need its own target value; its error signal describes how a small change to its preactivation would affect the final loss.

By reusing the error signals, we can avoid recomputing the same derivative products for different parameters.
##### Step 5: Gradient Descent
Now with all four gradients, we can update every parameter with the same rule:
```python
w1 = w1 - alpha * grad_loss_w1
b1 = b1 - alpha * grad_loss_b1
w2 = w2 - alpha * grad_loss_w2
b2 = b2 - alpha * grad_loss_b2
```
##### Step 6: Repeat
Repeat for remaining batches and epochs.
#### Branching
If an activation feeds into several units, sum the contributions:
![[Pasted image 20261114192927.png|491]]
With $\delta_u=\frac{dl}{dz_u}$ and $\delta_v=\frac{dl}{dz_v}$:$$\frac{dl}{da}=r\delta_u+s\delta_v$$
In this case, a change to a changes both $z_u$ and $z_v$, so to find the change in loss $l$ w.r.t. $a$, we **add** the effects of both paths. All contributions should be collected before continuing upstream.
#### Training with Backpropagation
Overall, the training steps are:
1. Initialize weights randomly and biases (usually as zero).  
2. Forward pass: compute activations and retain intermediate values.
3. Compute the loss for the current batch.
4. Backward pass: compute all parameter gradients (chaining derivatives),  using error signals to save on computations..
5. Gradient step: update the parameters using those gradients.
6. Repeat for remaining batches and epochs.

Backpropagation is step 4. The optimizer (gradient descent) does step 5. 
#### Matrix Form
The core idea remains the same; cache the forward pass, compute error signals backwards, turn each $\delta$ into parameter gradients. What changes is that every scalar is now a vector or matrix, and every multiplication is a matrix multiplication. Additionally, we now have branching, which means we must sum the contributions of each activation along different paths. Also, since each unit has its own sigmoid, the activation derivative is applied elementwise
#### From one Example to a Batch
In one example, everything is in a single row 