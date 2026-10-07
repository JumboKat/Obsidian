![[Pasted image 20260927230533.png]]**Linear regression** is a supervised learning model that makes use of **gradient descent**  as its training algorithm. This method of learning is used by almost everything in modern ML, including deep neural networks.
### History
In 1886, Sir Francis Galton introduced the idea of "regression," which focused on the relationship between the heights of children and their parents, observing that children's heights tended to regress towards the average. Important ideas in correlation and regression were later formalized by Karl Pearson.

The method of least squares was introduced even earlier in 1805 by Adrien-Marie Legendre. Carl Friedrich Gauss developed on his work further for astronomical data analysis.
### The Regression Problem Type
The problem that regression aims to solve is: given a dataset with examples labeled (x<sub>i</sub>, y<sub>i</sub>), where y<sub>i</sub> is a real number (rather than a class), predict y for a new x. Regression tasks include predicting housing prices, forecasting stock market, weather prediction, sales forecast, predicting battery life, etc.
#### Example: Old Faithful
![[Pasted image 20260926181106.png]]
This graph shows the relationship between the waiting time to next eruption and the previous eruption's duration. A linear relationship can be observed between the two: shorter eruption durations tend to precede shorter wait times, while longer eruption durations correlate with longer wait times.
### The Linear Model
A linear model asserts that the predicted value y<sub>i</sub> can be expressed as a linear combination of the feature values:
$$\hat{y}_i=\theta_0+\theta_1x_1^{(1)}+\theta_2x_i^{(2)}+...+\theta_Dx_i^{(D)}$$
Where:
- θ<sub>0</sub> is the **bias term** / intercept, because it shifts the prediction independently of the inputs.
	- Moves the regression hyperplane so that the model is not forced to pass through the origin
	- It is a constant offset, compensating for systematic effects not explained by the features
	- If all features are 0, $\hat{y}$ is always 0, which is a hardcoded assumption we want to avoid
	- ex: The relationship between eruption duration (x) and time to next eruption (y). $$\hat{y} = 32.910 + 10.503x$$ Even if x was hypothetically 0, the model predicts a wait time of about 33 minutes. Forcing the line through the origin would distort the fit in the region that does matter (between 1.5 and 5 minutes).
	- It is called bias because it is a fixed baseline to which the contributions of other parameters (weights and biases) are added.
- θ<sub>j</sub> is the jth parameter of the model (its feature weight)
- $\hat{y}$ and h<sub>θ</sub>(x) mean the same thing (h is the hypothesis function from a predefined hypothesis space, which encompasses the set of all possible models that can be used to predict outcomes based on input).
### Mean Squared Error (MSE)
The MSE is a common objective for regression problems. The model trains by searching for parameter values that minimize MSE. It is calculated by:
![[Pasted image 20260927171243.png]]
For each example, take the error (actual minus predicted) and square it, then average. Squaring makes all the errors positive and penalizes large errors much more than small ones. 

**RMSE** is the square root of MSE. Because the square-root function only increases, both MSE and RMSE have the same minimizing parameter values. It is preferred to minimize MSE over RMSE because the derivative is simpler. 
### Characteristics
A typical learning algorithm consists of the following components:
1. A **model**, often consisting of a set of **parameters** whose values will be learned.
2. An **objective function** that measures prediction error on the training data. This is commonly the MSE.
3. **Optimization** algorithm; **gradient descent** is commonly used and can be applied to a wide variety of models.
### Optimization
Optimization is a looping process that evaluates the loss function, comparing the hypothesis h with the target label y, then making small changes to parameters in order to reduce loss. This continues until an end criterion is met.

Parameter space is the set of all possible parameter combinations. For the model:$$h(x_i;\theta)=\theta_0+\theta_1x_1^{(1)}$$the parameter space $\Theta = \mathbb{R}^2$ would be the set of all possible (θ<sub>0</sub>, θ<sub>1</sub> ) pairs. The hypothesis space would be the set of all possible lines (functions of x) you could draw. Every point in parameter space corresponds to exactly one line in hypothesis space. E.g. θ = (5,2) gives h(x) = 5 + 2x. Gradient descent operates in parameter space. It adjusts the parameter θ to reduce the loss J(θ).

The notation h(x<sub>i</sub>;θ), which comes from statistics, indicates that the value of the function h depends on the input example x<sub>i</sub> and the parameters θ. The semicolon is used to semantically differentiates these two sets of values. We can also use the equivalent notation h<sub>θ</sub>(x<sub>i</sub>) in machine learning. This reads: "the function h, parameterized by θ, applied to the input x<sub>i</sub>."
### Derivatives
The derivative tells you the slope of the tangent line at any point (i.e. the instantaneous rate of change, or, the **gradient**). When the derivative function is negative (f'(t)< 0), the function is decreasing. When the derivative function is positive, the function is increasing. When the derivative is equal to zero, this is the minimum of the function, local or maximum. The sign indicates direction uphill the function is going, while the magnitude indicates how steep it is.

To achieve the minimum value of f(t), we look at the sign of f'(t). If the slope is positive, we want to move left (decrease t) to move toward the minimum. If the slope is negative, we want to move right. Gradient descent takes the derivative of the MSE J($\theta$) and finds the values of $\theta$ that sit at the minimum. 
#### Partial Derivatives
In linear regression, the MSE function J(θ) is dependent on multiple parameters/variables. Thus, we use partial derivatives; the derivative of J with respect to one parameter, while keeping all others constant. This lets us find the effect of the parameter on J in isolation:
![[Pasted image 20260927230700.png]]
Where $\alpha$ is the learning rate, which controls the size of each step. All parameter values are updated at the same time using their previous values.
#### Gradient Vector
The **gradient vector** is a vector containing the partial derivatives of J with respect to each parameter in D features:
![[Pasted image 20261001121652.png]]
This vector, which is the gradient, gives the direction of the steepest ascent. As the name **gradient descent** implies, we want to move in the opposite direction of this. This is why when we update parameters, this vector is subtracted:
$$\theta^{(t+1)}=\theta^{(t)}-\alpha∇_\theta J(\theta^{(t)})$$
### Convexity
A function is convex if for any two points on the graph of the function, the line connecting these two points lies above or on the graph. A convex function has no local minima, only a global minimum. The MSE for linear regression, along with logistic regression objectives, are convex in their parameters. Non-convex functions, like neural network training objectives, can have local minima, and so gradient descent may approach a local minimum instead of the global minimum.

For functions that lack a global minimum (etc. the cubic function), gradient descent can go on indefinitely, preventing convergence. This means that being differentiable on its own is not a sufficient condition to guarantee convergence. 
### Learning Rate
![[Pasted image 20261001182642.png|401]]
If the gradient tells us which direction to descend, learning rate ($\alpha$) tells us how big a step to take in that direction. Having a small learning rate is safe and can ensure you reach the minimum, but it means taking more iterations, which can be slow and wasteful. Having a large learning rate can cause you to overshoot the minimum, and actually start diverging away from the minimum.

Because the gradient shrinks as you approach the minimum (at which it is zero), a fixed learning rate will naturally produce smaller steps the closer you get to the minimum. 
### Example Size in Gradient Calculation
When calculating the gradient of a dataset, we do not necessarily need to use all of the examples. There are three approaches to the number of examples we use:
#### Batch Gradient Descent
This algorithm uses all N examples in the dataset for every single parameter update. This means the gradient is exactly accurate, and so the trajectory is smooth and direct.

However, each update is expensive, requiring the entire dataset to be processed. It also does not scale well as the number of examples grows; it is straight-up useless if the dataset doesn't fit in memory.
#### Stochastic Gradient Descent
This algorithm uses exactly one random example per update. This means that every update is insanely fast and cheap, and works naturally for very large datasets. The random selection aspect can also help reduce systematic bias caused by the order of the examples.

However, the trajectory is very noisy. Because the gradient calculation based off just one example is a very rough approximation of the true gradient, it can point in a direction that is different from the true downhill direction. Thus, it can scatter around the true direction. In non-convex objectives this may be helpful, as it can redirect the optimizer away from local minima or saddle points. This is random, however, and does not apply to convex objectives like linear regression's MSE. Additionally, SGD on its own tends to oscillate around the minimum, but never settling at the minimum. Since we are using a different random example each update, the next gradient is unlikely to be exactly zero. Thus, a **learning schedule** that reduces the learning rate close to the minimum is often used.
#### Mini-Batch Gradient Descent
This algorithm serves as the middle ground between the two previous methods. It takes a random subset of size B, where 1 < B < N. Its the common practical default because:
- **Computation efficiency**: matrix operations on a batch (e.g. 32 examples) can be vectorized and run efficiently on a GPU, which makes better use of hardware than just one example at a time.
- **Reasonable noise level**: there is enough averaging to avoid the worst zig-zagging, while remaining far cheaper per update than scanning the full dataset.

![[Pasted image 20261001190127.png]]
The choice on the algorithm to use is a matter of *optimization*, not necessarily *generalization*. In some experiments, models trained with smaller batches performed better on new data; the randomness of mini-batch training can influence the solution found even when no explicit penalty is added. This is called **implicit regularization**. This isn't a general rule, as large batches can perform just as well with a well-adjusted learning rate, training schedule, and training duration.