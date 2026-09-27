**Linear regression** is a supervised learning model that makes use of **gradient descent**  as its training algorithm. This method of learning is used by almost everything in modern ML, including deep neural networks.
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

