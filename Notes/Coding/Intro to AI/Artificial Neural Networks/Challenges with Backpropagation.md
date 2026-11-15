Backpropagation gives you gradients, but this is only part of the story.
1. Optimization can still go wrong:
	- A gradient is only local slope info, it can't see the big picture
	- Stationary points (zero gradient) can be minima, maxima, or saddles. Convergence != global optimality
	- The three toy landscapes: convex, nonconvex, and flat region
2. Vanishing vs. Exploding gradients
	- 
### Loss Landscapes and Optimization
Backpropagation was popularized in 1986, however deep learning didn't take off until much later. Backpropagation computes the gradients, explaining how the current loss changes when parameters change. This is only one part of learning, which requires an objective, effective updates, generalization on unseen examples, and retention when learning continues. A correct direction locally (zero or small gradient) does not necessarily lead to the best global solution. 

A real network has thousands to billions of parameters, so its loss landscape lives in that many dimensions. 
- A **local minimum** is a point that's at least as good as everything nearby.
- A **global minimum** is a point at least as good as every allowed parameter value; the lowest anywhere.
- A **stationary point** is any point where the gradient is zero. Gradient descent has no reason to move from this point.
The issue is that a stationary point can be a minimum (loss rises in all directions), a maximum (loss falls in all directions), or a saddle point. A **saddle point** is a point where the loss goes up in some directions and down in others (like the shape of a horse saddle; curved up front-to-back and curved down side-to-side). A 1D plot cannot show this since you only move in one direction.

**Convergence** means an algorithm settles toward some limiting value on each iteration, though this may not be the optimal value. The three loss landscapes below show this: the green point indicates the starting point, with the red indicating the end.

![[Pasted image 20261114202438.png]]
In the convex example, the algorithm converges to the global (and local) minimum. The nonconvex example converges on only a local minimum, and the third converges where it started, since it is on a flat region.
### Gradient Propagation
In backpropagation, the error signal at layer $l$ is computed from the signal at later $l+1$ by multiplying by a single number q = wₗ₊₁ · φ′(zₗ), which combines:
- the weight connecting the layers
- the activation's derivative at that unit.
So at each step towards the input, the signal gets multiplied b q. After d steps, starting from signal 1, the magnitude is $q^d$.
![[Pasted image 20261114210108.png]]
This graph shows the effect of different per-step multiplier values on the signal's magnitude. If the factors that make up the per-step multiplier are small (less than 1), the gradient shrinks with each step. Eventually, the gradient is so small that early layers barely learn (since the change in loss w.r.t. a change in the layer's parameters will be marginal). This is known as the **vanishing gradient** problem. Conversely, a large per-step multiplier (greater than 1) expands the gradient with every step, resulting in an **exploding gradient**. This is why parameter initialization is important to keeping training stable.
### Activation Functions and their Derivatives
![[Pasted image 20261114211732.png]]
An activation is **saturated** when changing its input produces very little change in its output, making its local derivative close to zero. An activation that can avoid saturation (low derivative value) keeps a backward signal useful

For a sigmoid, this generally occurs near either end of its output range. The sigmoid's derivative is <=0.25, and goes to zero when saturated, so it can contribute to the vanishing gradient problem.

Tanh's max derivative is 1 at 0, but it also saturates as you move away from the origin. 

ReLU's derivative is 1 for positive preactivations and 0 for negative ones. It avoids saturation for positive values, but not for negative values. 
#### Dying ReLU Problem
**Sparsity** means that many entries are exactly zero. ReLU produces sparse activations because it outputs zero for non-positive inputs (including at zero). The activations are sparse, meaning for any given input, many hidden units output 0. This does not necessarily mean that the weights are zero. 

During backpropagation, the signal passing through that unit is multiplied by its activation derivative. Since ReLU's derivative is 0 for z < 0, this means the signal becomes zero. No update happens through that unit. For a single example, this is fine since other examples can still update the unit. The problem arises when a unit is inactive for all training inputs. This means that:
- its derivative is 0 on every example
- It receives no data-loss gradient
- its weights stop being updated
- nothing pushes z back above 0, and it stays inactive ("dead")

The unit no longer contributes to the network. It is possible for dead units to revive if an earlier layer's weights change and shifts its z to become positive. This is the **Dying ReLU Problem**
#### Leaky ReLU
Furthermore, ReLU's derivative of 1 on the positive side avoids the shrinking effect of the sigmoid, but there are still other sources of attenuation (incoming activations, downstream weights, etc.), so ReLU helps, but does not solve the vanishing gradient.

![[Pasted image 20261114213137.png]]
The **Leaky ReLU** mitigates the dying ReLU problem by using a small slope $\alpha$ for negative inputs. This makes the negative side's derivative non-zero, allowing a unit with z < 0 to receive some gradient, allowing it to recover instead of dying.
### Initialization
Weights must be randomized. If two hidden units have identical initial parameters and the same role, they compute the same output, receive the same gradient, and get the same update. They stay identical forever, and essentially both behave like one unit. This is the **symmetry problem**. Randomizing the weights breaks this symmetry, ensuring that each unit can learn a different feature. Biases can start at zero, since the random weights already distinguish each unit.
#### Scaling Weights
Because weights are a component of the per-step multiplier, their initial size affects whether signals shrink or grow as they pass through layers:
- Too small: activations and gradients shrink (vanishing)
- Too large: units saturate, and signals can blow up (exploding)

A good initialization tries to start q around 1 on average, keeping the signal's size reasonable in both directions.

![[Pasted image 20261114214004.png]]
![[Pasted image 20261114214009.png]]
The graphs show the effect on activation value and gradients with standard initialization (top) and normalized initialization (bottom). With normalized initialization, the distributions are roughly the same across all five layers.

**Xavier/Glorot initialzation**: $Var(W)=\frac{2}{n_{in}+n_{out}}$
- $n_{in}$ and $n_{out}$ are the layer's number of inputs and outputs
- Keeps the forward signal (depends on $n_{in}$) stable as well as the backward ($n_{out}$ )
- Used for sigmoid and tanh layers