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
### Loss Function
![[Pasted image 20261003174457.png|339]]
The **Bernoulli Distribution** models a distribution of either yes (1) or no (0). For binary outcomes, the probability is given by:
$$P(y|x,\theta )=\sigma (\theta ^\top x)^y(1-\theta ^\top x))^{1-y}$$Since y can only be 0 or 1, one factor always has the exponent zero and is simplified to 1. The other selects the probability assigned to the predicted label. Taking the **mean negative log-likelihood**: 
$$J(\theta ) = -\frac{1}{N}logL(\theta )=-\frac{1}{N}\sum_{i=1}^NlogP(y_i|x_i,\theta)=-\frac{1}{N}\sum_{i=1}^Nlog[\sigma (\theta ^\top x_i)^{y_i}(1-\theta ^\top x_i))^{1-y_i}]$$

This is the **loss function** J(θ) that we aim to minimize.
### Cross-Entropy
**Entropy** measures the uncertainty in a random outcome. An event with a guaranteed outcome has an entropy of zero.

**Cross-entropy**, given by H(p, q), is the average penalty you pay when the model prediction is wrong (p is the true distribution but q was predicted). So, **binary cross-entropy (BCE)** = **log loss** = mean negative log-likelihood; they are all terms for the same thing. Again, because y is either 0 or 1, only one term is active per example. If y - 1m the loss is -logp̂. 
- As p̂ → 1 (confident and correct), loss → 0.
- As p̂ → 0 (confident and wrong), loss → ∞.
This means BCE punishes confidently wrong answers more harshly.
#### BCE vs MSE
![[Pasted image 20261003183337.png|614]]
Why is BCE used over [[Linear Regression#Mean Squared Error (MSE)||MSE]] with logistic regression? 
##### Gradient
Given linear score z = θ<sup>⊤</sup>x and probability p̂ = σ(z), we want to find how an update to the parameters (changing z) affects the loss. The gradient (rate of change of the loss) of the two loss functions are as follows:
- **MSE gradient**: ∂ℓ/∂z = 2(p̂ − y) · p̂(1 − p̂)
- **BCE gradient**: ∂ℓ/∂z = p̂ − y

Take a positive example where the model is confidently wrong (y=1 but p̂ ≈ 0).  
- MSE: the error factor (p̂ − y) is about -1, but the positive factor p̂(1 − p̂) is about 0. The product is near-zero, so the gradient vanishes and the loss **saturates**. This means that the model learns slowly when it is wrong (the loss is not much higher if the model is more confidently wrong).
- **BCE**: (p̂ − y) gives -1, so the loss is harshest when the model is confidently wrong. When the model is confidently right, the loss tends to zero.
##### Loss matches the data type
The model produces a binary outcome, and so does the loss function. Outputs are meaningful probabilities; minimizing the BCE pushes p̂ toward the true frequency of class 1, so if the model says 0.8 for a group of examples, about 80% of them should be positive. 

On the other hand, MSE does not always correlate to a maximum likelihood; it works for a continuous prediction.
##### BCE is convex
For BCE with a sigmoid, J(θ) is convex. For MSE with a sigmoid, the loss function is not convex, but flattens at both ends; it is not globally convex, and so there are regions where gradient descent will stall (where it flattens). MSE is convex for linear regression; it is only incompatible with the sigmoid function.
### Geometric Interpretation
A linear score t is computed as:
$$t(x)=\theta_0+w^\top x$$
- w is the vector of feature weights
- θ<sub>0</sub> is the intercept (bias)
- $w^\top x=||w|| ||x||cos\phi$ (is a dot product)
