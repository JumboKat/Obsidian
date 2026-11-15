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
