Raw data is rarely fit for use by a model. It needs to be transformed first. This includes encoding, scaling, imputing, rebalancing, and feature engineering. 

Every transformation that learns something must learn it from the training data only, ideally within a **Pipeline**. We want to process the data without leaking it at each step.
### High-Quality Data
For data to be high-quality (fit for model use), it must have the following characteristics:
- **Volume**: large enough to support generalization.
- **Relevance**: features accurately model the problem.
- **Representation**: the dataset has features that are obtainable in real-world scenarios and adequately covers feature examples.
- **Bias**: dataset is unbiased.
- **Label consistency**: labels are consistent throughout the dataset.
### Encoding
The method of encoding numerical features should match the data's meaning. It should preserve whether categories are unordered or have inherent order. 
#### One-Hot Encoding
The common default for unordered (**nominal**) categories. A categorical variable is represented by a binary vector, where each category is represented by a '1' for its respective element, with all others being '0'.
- **Increases dimensionality**: creates one output feature per retained category.
- **Avoids artificial similarity and order** : integer codes can introduce distance and order where there should be none.
	- ex: `w = 1, 2, 3, etc...` implies features 1 and 2 are similar, while 1 and 3 are not.
	- ex: `w = [0,0,0], [0,0,1], etc...` may suggest similarity based on numeric patters (the first two numbers are the same, so the features are related).

Sci-kit Learn provides OneHotEncoder. Ensure you use the same encoding on the validation set, test set, and production data.

An encoder learns categories and their column order during `fit`. Fitting an encoder a second time on any set can have a different number of categories or a different order. The correct method is to fit the encoder on the training data once, and reuse it everywhere else. A pipeline does this during cross-validation.

Pandas offers a method called `get_dummies()` for one-hot encoding, but there are some differences:
- **Category memory**: `OneHotEncoder` remembers the categories it was trained on, `get_dummies()` does not. It builds columns from the data handed to it each time, so it suffers the problem mentioned above. 
- **Handling missing values**: `get_dummies()` encodes a missing value as all zeros by default, with an explicit missing-value column created with `dummy_na=True`.
#### ColumnTransformer
Ensuring consistency in encoding across training, validation, and production datasets is essential.
```python
# Apply OneHotEncoder and StandardScaler using ColumnTransformer 
preprocessor = ColumnTransformer(    
	transformers=[
		(
			'onehot',
			OneHotEncoder(drop='first', handle_unknown='ignore'),
			columns_to_encode,        
		),        
		('scaler', StandardScaler(), numerical_columns)    
	] 
)
```
Sci-Kit Learn's `ColumnTransformer` is used to apply different transformations to select columns. It's transformer can also be `drop`, which excludes the selected columns, or `passthrough`, which leaves them unchanged. 

```python
# The pipeline fits preprocessing and the classifier on training data only
heart_pipeline = Pipeline([
	('preprocessor', preprocessor),
	('classifier', LogisticRegression()),
])
heart_pipeline.fit(X_train, y_train)
```
A `Pipeline` is an engineering pattern that surrounds the column transformer. calling `fit` learns both the preprocessing parameters and the classifier from the training data. Calling `predict` applies the already-fitted transformations before classifications. during CV, each fold learns its own peprocessing parameters from that fold's training partition.

#### Ordinal Encoding
**Ordinal encoding** assigns numerical values to categorical attributes based on their inherent order or rank. It preserves ordinal relationships among categories. It is also more compact than one hot encoding. However, if there is uncertainty about the ordinal nature of the data, `OneHotEncoder` is the safer option.
```python
from sklearn.preprocessing import OrdinalEncoder

X = [['Poor'], ['Average'], ['Good'], ['Average'], ['Average']]

encoder = OrdinalEncoder(categories=[['Poor', 'Average', 'Good']])
encoder.fit(X)

X_encoded = encoder.transform(X)

"""X_encoded looks like:
array([[0.], [1.], [2.], [1.], [1.]])
"""
```
We must explicitly provide the category order we want to the encoder as shown in the example above. Otherwise, it defaults to alphabetical order.
#### Binning
A form of **discretization**, where continuous or fine-grained numerical features are mapped into a finite set of intervals ("bins"). 
- ex: Categorizing ages into bins like 'infant', 'child', 'teen', 'adult', 'senior citizen'.

Binning encodes useful domain thresholds and can help simplify the model. For instance, it can help decision trees produce a more compact model because they no longer have to discover every threshold from the sample. However, it removes differentiation between points within the same interval, and introduces abrupt boundaries. The boundaries themselves may also be arbitrary.
### Scaling
**Scaling** means to make values fall within comparable ranges. This is required by some estimates that use distance, like [[K-Nearest Neighbours (KNN)]]. In these cases, large-scale features can dominate the distance. Gradient-based optimization often converges more readily when features are scaled.

Since tree-based models make threshold comparisons for one feature at a time, scaling has no effect (relative thresholds stay the same). 

For a fair ML workflow, scaling parameters are computed on training data only, then used to transform both train and test subsets. Again, scaling and regression occurs within the same pipeline. When values aren't scaled, the nearest neighbours, and subsequently the model's prediction, is different. Scaling ensures fair distance weighting; it may change the neighbour and prediction, but it does not improve the model's accuracy.
#### Scaling Methods
**Min-max scaling** maps each feature to a chosen interval, usually \[0, 1]. It is useful when a bounded input range is required.

**Standardization** subtracts the training mean and divides by the training standard deviation:
![[Pasted image 20261112160957.png]]
The transformed training feature has mean 0 and variance 1, but its distribution does not become normal and its range is not bounded. Standardization is a commonly used for distance-based methods, regularized linear models, and gradient-based optimization.

Both methods are sensitive to outliers; min-max scaling uses extremes, while standardization uses mean and S.D. For strongly-skewed features or outliers, `RobustScaler` uses the median anad IQR, which is less sensitive to extreme values.

![[Pasted image 20261112161242.png]]
#### Data Leakage
To avoid data leakage, follow this workflow:
- Split into training and test sets before learning any preprocessing parameters.
- Fit the scaler on the training data only.
- Reuse the same scaler to transform validation, test, and production data.
- During CV, fit preprocessing separately inside each training fold, usually by placing it in a `Pipeline`.

Means, S.D.s, extremes, category sets, and imputation values are all learned information and must come from the training data only. The test set represents unseen data. Allowing it to influence preprocessing gives the training procedure information it would not normally have on genuine new data. Data leakages can produce an optimistic estimate on generalization performance.