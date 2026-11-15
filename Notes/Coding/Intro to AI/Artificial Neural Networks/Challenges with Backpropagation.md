Backpropagation gives you gradients, but this is only part of the story.
1. Optimization can still go wrong:
	- A gradient is only local slope info, it can't see the big picture
	- Stationary points (zero gradient) can be minima, maxima, or saddles. Convergence != global optimality
	- The three toy landscapes: convex, nonconvex, and flat region
2. Vanishing vs. Exploding gradients
	- 
### Loss Landscapes and Optimization
Backpropagation was popularized in 1986, however deep learning didn't take off until much later. Backpropagation computes the gradients, explaining how the current loss changes when