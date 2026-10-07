### The Holdout Method
Cross-validation is a method used to evaluate the performance of a model by splitting the dataset into training subsets and a testing subsets. It estimates how well a model will perform on unseen samples.

The **Holdout Method** is the simplest form of splitting, creating one training subset and one testing subset (usually 80-20).
##### Considerations for Choosing Split Ratio
1. Dataset size: there should be enough samples in the test subset to train on. With a larger dataset, we can dedicate a greater proportion of samples to the training split.
2. Model complexity: complex models with many parameters may require more training samples to avoid overfitting.
3. Validation data: training data can be reused for validation; separate validation set may be preferred when computational costs or data structure makes cross-validation impractical
4. Imbalanced datasets: both splits must contain enough examples from each class. **Stratified splitting** can approximately preserve class proportions, but does not correct class imbalance itself.

**Training error** is the error measured during training. Model fitting seeks to reduce training loss. A low training loss does not guarantee a low **generalization** **error** (prediction error on new examples). Generalization error can't be observed directly and must be estimated through validation and testing.

#### Underfitting and Overfitting
**Underfitting** is represented by high training error, it means the model is failing to learn underlying patterns. It is too simple and performs poorly on training and test data. **Overfitting** is represented by low training error, but high generalization error. The model is capturing too much noise and/or irrelevant patterns. It 'memorizes' the training data, and performs well on it, but poorly on new data.
### K-Fold Cross-Validation
1. Split training data into k equal folds
2. Repeat k times: start with a new, unfitted classifier, train on k-1 folds, evaluate on remaining fold.
3. Summarize the k scores (mean and standard deviation)

Commonly, k is chosen to be 5 or 10. 
![[Pasted image 20261006235611.png]]

 Each iteration produces a distinct fitted classifier; nothing learned carries over. K-fold CV evaluates a model-building procedure, not one persistent classifier. It doesn't improve generalization itself, but rather helps you compare classifier choices using only training data (test subset is never used). The fold scores are not independent; with 5 folds, any two classifiers share 3 complete folds. 

The **standard deviation** describes variation, not standard error.

CV is useful when data is limited. Once a model is selected, the chosen procedure can be refitted on the complete training set.
#### Challenges
- Computational cost: requires repeated model fitting.
	- **Leave-one-out (LOO)**: is the extreme case where (k = N)
- Class imbalance: random folds may not represent minority classes
	- use stratified cross-validation to approximately maintain class proportions.
- Implementation complexity: 
### Data Leakage
Data leakage occurs when data from the test subset influences training. This occurs if fitting is done on a dataset before it is split, or if the dataset is not split at all:
```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)   # sees ALL folds
scores = cross_val_score(KNeighborsClassifier(), X_train_scaled, y_train, cv=cv)
```
