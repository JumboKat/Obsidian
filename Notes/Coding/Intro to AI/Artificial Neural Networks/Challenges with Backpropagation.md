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
#### Activation Functions and their Derivatives
Choosing the right activation function can help propagate gradients.
![[Pasted image 20261114211732.png]]
An activation is **saturated** when changing its input produces very little change in its output, making its local derivative close to zero. An activation that can avoid saturation (low derivative value) keeps a backward signal useful

For a sigmoid, this generally occurs near either end of its output range. The sigmoid's derivative is <=0.25, and goes to zero when saturated, so it can contribute to the vanishing gradient problem.

Tanh's max derivative is 1 at 0, but it also saturates as you move away from the origin. 

ReLU's derivative is 1 for positive preactivations and 0 for negative ones. It avoids saturation for positive values, but not for negative values. 
##### Dying ReLU Problem
**Sparsity** means that many entries are exactly zero. ReLU produces sparse activations because it outputs zero for non-positive inputs (including at zero). The activations are sparse, meaning for any given input, many hidden units output 0. This does not necessarily mean that the weights are zero. 

During backpropagation, the signal passing through that unit is multiplied by its activation derivative. Since ReLU's derivative is 0 for z < 0, this means the signal becomes zero. No update happens through that unit. For a single example, this is fine since other examples can still update the unit. The problem arises when a unit is inactive for all training inputs. This means that:
- its derivative is 0 on every example
- It receives no data-loss gradient
- its weights stop being updated
- nothing pushes z back above 0, and it stays inactive ("dead")

The unit no longer contributes to the network. It is possible for dead units to revive if an earlier layer's weights change and shifts its z to become positive. This is the **Dying ReLU Problem**
##### Leaky ReLU
Furthermore, ReLU's derivative of 1 on the positive side avoids the shrinking effect of the sigmoid, but there are still other sources of attenuation (incoming activations, downstream weights, etc.), so ReLU helps, but does not solve the vanishing gradient.

![[Pasted image 20261114213137.png]]
The **Leaky ReLU** mitigates the dying ReLU problem by using a small slope $\alpha$ for negative inputs. This makes the negative side's derivative non-zero, allowing a unit with z < 0 to receive some gradient, allowing it to recover instead of dying.
#### Initialization
Weights must be randomized. If two hidden units have identical initial parameters and the same role, they compute the same output, receive the same gradient, and get the same update. They stay identical forever, and essentially both behave like one unit. This is the **symmetry problem**. Randomizing the weights breaks this symmetry, ensuring that each unit can learn a different feature. Biases can start at zero, since the random weights already distinguish each unit.
##### Scaling Weights
Because weights are a component of the per-step multiplier, their initial size affects whether signals shrink or grow as they pass through layers:
- Too small: activations and gradients shrink (vanishing)
- Too large: units saturate, and signals can blow up (exploding)

A good initialization tries to start q around 1 on average, keeping the signal's size reasonable in both directions.

![[Pasted image 20261114214004.png]]
![[Pasted image 20261114214009.png]]
The graphs show the effect on activation value and gradients with standard initialization (top) and normalized initialization (bottom). With normalized initialization, the distributions are roughly the same across all five layers. 

There are two kinds of initialization:
**Xavier/Glorot initialzation**: $Var(W)=\frac{2}{n_{in}+n_{out}}$
- $n_{in}$ and $n_{out}$ are the layer's number of inputs and outputs
- Keeps the forward signal (depends on $n_{in}$) stable as well as the backward (depends on$n_{out}$)
- Used for sigmoid and tanh layers

**He initialization**: $Var(W)=\frac{2}{n_{in}}$
- Used for ReLU hidden layers
`SimpleMLP` applies He init to ReLU hidden layers, and Xavier/Glorot to everything else. `SimplerMLP` uses Xavier/Glorot throughout.

Initialization helps signal propagation, but does not guarantee convergence or optimize performance. It also only controls the starting point; training can still be destabilized later by large updates.
#### Gradient Clipping
**Gradient clipping** caps how big a single update step can be without changing its direction. For a chosen threshold $c$, with $g$ being the gradient vector:
- if $||g|| ≤ c$, no changes
- if $||g|| > c$, rescale it to have length exactly c ($g=c*g/||g||$)
Since it is only multiplied by a positive scalar, the direction is unchanged. An update step then has a norm of at most a\*c.

Clipping helps prevent exploding gradients, but it does not solve the vanishing gradient, nor does it guarantee convergence. 
### Output Layers
While we can replace a hidden activation (sigmoid) with a ReLU, replacing the output's activation has an additional challenge: the output must match the target's dimensions and range.

Activation functions in the hidden layers are chosen to improve performance and convergence, while the activation function in the output layer is chosen to match the nature of the prediction task:
- Regression: linear, ReLU, or softplus, depending on range constraints. 1 output per dimension.
- Binary classification: sigmoid; 1 output.
- Multi-class classification: separate sigmoids; 1 output per class.
- Multi-label classification: softmax; 1 output per label. 

Examples of multi-output regression problems:
2 outputs:
- predicting latitude and longitude of a location from an image.
- estimating with and height of a bounding box in object detection
- forecasting temperature and humidity simultaneously.
3 outputs:
- predicting the x, y, z coordinates of a point in space.
- estimating RGB colour values from spectral measurements.
4 outputs:
- predicting bounding box coordinates (xmin,ymin,xmax,ymax) in object detection.
#### Multi-label Predictions
![[Pasted image 20261115161102.png|411]]
Multi-label predictions produce independent **sigmoid** outputs for a single example (multiple labels apply simultaneously), and their probabilities do not need to sum to one. 
#### Multi-class Predictions
![[Pasted image 20261115161228.png|428]]
A multiclass prediction is where the model predicts one class out of a pool of mutually exclusive classes for an example. Only exactly one class can be correct. 

An output **logit** is the raw score before the final activation converts it into a probability (in other words, the output layer's preactivations). 

![[Pasted image 20261115162913.png|353]]
The **softmax** function is used for multiclass predictions. It ensures that all activation outputs fall between 0 and 1 (non-negative) and collectively sum to 1, allowing the outputs to be interpreted as probabilities. It preserves relative ordering, with the largest logit corresponding to the largest probability. Bigger gaps between scores gives the winner a bigger share of the overall probability, while small gaps give a near-even split. With softmax, only the differences between outputs matters. Adding a constant to every logit changes nothing, since the relative differences stay the same.
##### Overflow and Underflow
If logits are too large (e.g. 1000), the softmax computation $e^{1000}$ **overflows**. The solution is to subtract all outputs by the largest logit first. This works since only the relative differences matter, not the raw magnitudes. **Underflow** is the opposite problem, where a tiny probability can round exactly to 0, and since cross-entropy loss takes the log of the probability, log(0) is undefined. Use **log-sum-exp** for stable loss.
##### from_logits
If the model already applies softmax, but you set `from_logits=True` for the output softmax, the loss applies softmax a second time to values that are already probabilities. Only set `from_logits` if the last layer does not already apply softmax.
##### Argmax
The **argmax** is an alternative that simply picks the class with the largest score. While it is good for the final decision, it is terrible for training. Unless it changes the overall winner, a small change in the scores does not affect the score at all. This means the derivative is almost zero everywhere. At the point where the winner does flip, it manifests as a sudden jump, where the derivative is undefined. Argmax gives zero or nothing, which clashes with how gradient descent facilitates learning, which needs a smooth signal that indicates a direction to move toward. Softmax fulfils this. 

During training, use softmax. After training, apply argmax to the softmax output for class predictions. 
##### Softmax Limitations
The fundamental problem with softmax is that it distributes probability among only known classes; there is no option for "none of these". An example from a class absent from the training tasks is called an **out-of-distribution** input. It can receive a high probability for a known class. While there are ways to reject such outputs (e.g. requiring the largest probability to meet a threshold), it is still possible for unknown inputs to receive high probabilities and avoid rejection.
### Regularization
**Regularization** encompasses any modification to an algorithm that reduces its generalization error. Overfitting can still occur even after an appropriate loss and effective parameter updates.

**Training loss** only measures how well a model fits to the supplied examples. **Validation performance** helps identify choices that generalize. Training and validation loss can often diverge, and so reducing only training loss is not sufficient. Regularization influences learning to favour generalization. Methods can include:
- Stopping early to control how long parameters are adjusted.
- Penalties to change the objective.
- Dropout changes training computations.
If regularization is too strong, it can cause underfitting.
#### Early Stopping
![[Pasted image 20261115165437.png|420]]
A **checkpoint** is a saved copy of the model's parameters for a specific training step. It is sometimes favourable to stop a procedure early and keep an early checkpoint of the model at its lowest validation error rather than at its final epoch, despite continuing improvement in training error.

We can choose when to stop by monitoring a validation metric, and stop after a number of epochs pass without meaningful improvement and restore the best parameters. Set `restore_best_weights=True` as it is not default in Keras.
#### Penalties
Training normally only minimizes data loss. We can change the objective so that training also prefers small weights:$$J_{reg}(W,b)=J_{data}(W, b)+\Omega(W)$$
- $J_{data}$ is how well the model fits the task
- $\Omega(W)$ is the **penalty**, which grows when the weights are big (not biases)
- Training minimizes both together; it must balance fit against weight size.
##### Measuring Weight Size
There are multiple ways to measure weight size. We can either **sum absolute values (L1)** or **Sum the squares (L2)**. For example, take the two weights (4,0) and (2,2).
- L1 sees them as equal (4 = 2 + 2)
- L2 prefers (2,2), as squaring them punishes one large entry more than two smaller ones.
L2 favours spreading weight evenly, while L1 ignores distribution.
##### Norm (Length)
A **norm** assigns a non-negative 'length' to a vector:
![[Pasted image 20261115171506.png]]
- A norm is non-negative and is zero only for the zero vector. 
- Scaling a vector by c scales its norm by |c|.
- The triangle inequality: ‖u + v‖ ≤ ‖u‖ + ‖v‖
##### $ℓ1$ and $ℓ_2$ Regularization 
The combined objective:$$J_{reg}=J_{data}+\lambda_1||w||_1+\frac{\lambda_2}{2}||w||_2^2$$
The L1 term sums absolute weight values; the L2 sums squared weight values.
**L1 Regularization:**
- Can promote sparsity by pushing weights to exactly zero.
- Supports feature selection, especially in linear models.
**L2 Regularization**:
- Shrinks all weights, usually stays dense (few or no zeroes).
- Promotes stability, small distributed weights.
#### Dropout
**Dropout** randomly sets some activations to zero temporarily during training. The goal is to reduce overfitting by discouraging the reliance on particular units always being available. Repeating this for different units forces the model to make predictions without relying too heavily on certain combinations of units. While training loss may increase, the goal is to increase validation performance by improving generalization. 
##### Scaling
![[Pasted image 20261115174933.png]]
The **dropout rate** $p$ is the probability that an activation is dropped, with the probability of it being retained being $1-p$. For an activation that equals 4 before dropout and is dropped half the time, the average value is 2. Dividing each retained activation by its retainment probability ($1-p=0.5$) doubles it, lifting its average to 4. This **scaling** compensates for the activations that were dropped. 

In general, an activation $a$ = 0 when dropped, and $a/(1-p)$ when retained. Scaling keeps the activation magnitude comparable to ordinary use. 

Activations should not be dropped during inference. Because retained activations were scaled during training, no additional dropout scaling is needed at inference. Keras automatically handles this distinction:
- Dropout is active for training inside `model.fit`
- Inactive for validation, `model.evaluate`, and `model.predict`
### Inference and Further Learning
- **Further learning** means updating parameters.
- **Online learning** 
- **Fine-tuning**: further training an already-trained model.
- **Continual learning** studies from successive data/tasks while retaining earlier capabilities
- 