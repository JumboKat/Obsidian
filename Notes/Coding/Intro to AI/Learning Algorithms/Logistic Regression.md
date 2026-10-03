**Logistic Regression** is a a method used for [[Classification Tasks#Binary Classifcation||binary classification]] tasks (it is not a regression technique, as its name might imply). The labels are binary values (y<sub>i</sub> ∈ {0, 1}), and the objective of logistic regression is to determine the probability that a given instance x<sub>i</sub> belongs to the positive class (y<sub>i</sub> = 1). In this way it is different from [[Linear Regression||linear regression]], which predicts a continuous value.
### Logistic Regression vs Linear Regression
![[Pasted image 20261002155523.png]]
For classification tasks, linear regression does not always fit the data. In the example above, while the line produced by linear regression sort of works (it shows that longer flipper length correlates with Gentoo penguins), it does not accurately model the probability that an individual is a Gentoo or not. Additionally, the outputs can go above 1 and below 0, which is meaningless when talking about probability.
### Logistic Function
In math, the standard logistic function (called the **sigmoid function**) is an s-shaped curve that maps a real-valued input to the open interval (0, 1). It is defined as: $$\sigma (t)=\frac{1}{1+e^{-t}}$$
![[Pasted image 20261002153806.png]]
The curve is called a **squashing function** because it squishes a wide input domain to a small output range of 0 to 1. Like with linear regression, logistic regression computes a weighted sum (score) of the input features, but passes it through the sigmoid function, which maps all outputs to fit within the range of (0, 1). Thus, the probability is given by:
![[Pasted image 20261002154639.png]]
- t = 0 gives exactly 0.5
- large positive t gives a value near 1
- large negative t gives a value near 0
- The coefficients show which features push the score up or down.

The model, in its vectorized form, is given by:
![[Pasted image 20261002155248.png]]
### Predictions and the Decision Boundary
If the probability is greater than or equal to 0.5, we predict class 1; otherwise predict class 0. Since the **decision boundary** is where the score is 0, the model is 50/50 about the prediction here. The further away the model is from the boundary, the more confident it is in its prediction.
### Likelihood
The an objective of logistic regression is to maximize its confidence in its predictions. The **likelihood** is the measure of how much probability the model, with a given θ, assigned to the true label across the whole dataset. The **likelihood function** is the product of each example's probability:
$$L(\theta )=P(y_1|x_1)\times P(y_2|x_2)\times ...\times P(y_N|x_N)$$
**Maximum Likelihood Estimation (MLE)** means finding the parameter θ that makes the likelihood L(θ) as large as possible. There are two tricks that are used to help the optimization:
- Taking the log turns the product into a sum (the log of a product is the sum of its logs), which is easier to work with and does not change where the maximum is.
- Since optimizers typically try to minimize (loss), we can flip the sign and minimize the negative log-likelihood instead.
#### Bernoulli Distribution
![[Pasted image 20261003174457.png|339]]
The **Bernoulli Distribution** models a distribution of either yes (1) or no (0). For binary outcomes, the probability is given by:
$$P(y|x,\theta )=\sigma (\theta ^\top x)^y(1-\theta ^\top x))^{1-y}$$
- Since y can only be 0 or 1, one factor always has the exponent zero and is simplified to 1. The other selects the probability assigned to the predicted label.