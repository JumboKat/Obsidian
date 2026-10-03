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
### Minimizing Loss
The an objective of logistic regression is to maximize its confidence in its predictions. The **likelihood** is the measure of how much probability the model, with a given θ, assigned to the true label across the whole dataset. 