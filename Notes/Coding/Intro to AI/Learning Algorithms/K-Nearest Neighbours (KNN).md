KNN is a classification/regression method that differs greatly from a decision tree. To classify a new example, look at the **k** nearest points to it in the data and let them "vote" on what the new example's label will be. Whatever class is most common among these neighbours (i.e. whichever vote wins) becomes the prediction. This algorithm is built on the assumption that points near each other in feature space tend to share the same label.

KNN is instance-based, meaning it doesn't learn a compact set of parameters or rules like other models, it just stores the training examples themselves. In other words, the training set is the model. It is also non-parametric: there are no parameters that get fit during training, meaning no weights, coefficients, or tree structure. Non-parametric does not mean assumption-free: we still assume that proximity is meaningful and nearby examples tend to have similar targets.
### Learning
KNN is called a "lazy" algorithm because the learning step is simply just storing a copy of the training set. Retaining a copy requires O(ND) memory for N examples with D features. 
### Finding the Nearest Neighbours
We find the Euclidean distance from the k nearest neighbours, which costs O(ND + NlogN) per query. Computing all distances costs O(ND) and sorting them costs O(NlogN). 
### Voting and Averaging
After finding the k nearest neighbours, the label of the new example is calculated based on the task:
- Classification: each neighbour votes for its class.
- Regression: average the neighbours' numerical targets.

We can also choose to apply weighting to the neighbours or not. Applying **uniform** weights means every neighbour has the same influence, while **distance** weights are directly tied to how close neighbours are (weight = 1/distance). 
### Limitations
- Prediction retains and searches the training data.
- Distances are sensitive to feature scaling and the chosen metric (some features may be more varied than others, which can cause them to have more influence on the distances).
- Curse of dimensionality: as the number of dimensions increases, the notion of distance loses value.
- Small k can be sensitive to noise; large k can hide local structure.
- Class imbalance can skew a neighbourhood's vote.