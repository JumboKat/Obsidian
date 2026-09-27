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
- θ<sub>0</sub> is the bias (shifts the line/hyperplane so it doesn't pass through the origin)
- θ<sub>j</sub> is the jth parameter of the model (its feature weight)
- $\hat{y}$ and h<sub>θ</sub>(x) mean the same thing ("the hypothesis h, parameterized by θ, applied to input x)