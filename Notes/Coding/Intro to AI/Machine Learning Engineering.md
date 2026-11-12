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
### Imputation
**Missing values** are data points or entries missing when it is expected. 
- ex: some patients withhold their age for privacy reasons.
#### Handling Missing Values
There are many ways we can handle missing values:
- **Drop Examples**: drop an entire row that contains one or more missing values. Reasonable as long as enough data is left over and missing rows are not systematically different.
- **Drop Features**: fine if a feature is not essential and there isn't enough reliable information anyway.
- **Use Algorithms**: algorithms that tolerate missing values (etc. model or drop them)
- **Data imputation**: replace missing values using statistics or a model learned from the training data.
#### Data Imputation
##### Univariate Imputation: Using Mean or Median
```python
from sklearn.impute import SimpleImputer

X_train_missing = np.array([
	[1.0, 10.0],
	[2.0, np.nan],
	[3.0, 12.0],
])
X_new_missing = np.array([[4.0, np.nan]])

imputer = SimpleImputer(strategy="median")
imputer = fit(X_train_missing) #learn fill values from dataset
imputer.transform(X_new_missing) #impute median values
# New X_new_missing becomes array([[4.0, 11.0]])
```
This method is simple, but it ignores relationships between features and reduces variability. For categorical features, we use the most frequent category or a special "missing" category.

Missing data can be:
- **MCAR (Missing Completely at Random)**
- , **MAR (Missing at Random)**: Missingness is related to other features (e.g. younger survey participants are more likely to skip a mental health question, but you do have their age recorded in the dataset. Accounting for age, missingness within age groups is random).
- **MNAR (Missing Not at Random)**: Missingness is related to the missing values themselves (e.g. patients drop out of a clinical trial because their side effects are severe)

A simple imputer cannot determine which mechanism produced the missing values. Mean, median, and most-frequent imputation serve as baselines, but can distort distributions and relationships. As with scaling, imputer should only be fit on the training partition. A pipeline then repeats the fit correctly within every CV fold.
##### Preserving Missingness Information
We can accompany an imputed value with a binary feature indicating that the original value was missing:
```python
imputer_with_indicator = SimpleImputer(
	strategy="median", add_indicator=True
)
imputer_with_indicator.fit_transform(X_train_missing)
```
##### Model-based Imputation: Predicting a Missing Feature
Set a feature with missing values as the prediction target and rows with that feature present as the training examples. Other observed features are inputs. Then use a regression or classification model to predict missing entries.

There are two main methods:
- **Neighbour-based method**: KNN finds the k closest examples and impute using the non-missing values from the neighbourhood.
- **Iterative model-based methods**: predict each incomplete feature based on the present features; repeat the process across features.

Unlike univariate imputation, these methods use relationships between features to predict missing values; it isn't guaranteed to be better.
### Resampling: Remedying Class Imbalance
Class imbalance can hide poor performance on a rare class. How can the training procedure respond to class imbalance without interfering with evaluation?
#### Training-Time Remedies (Resampling)
- **Class or sample weights**: make errors on select examples (of the rare class) more corrective toward the training objective.
- **Random oversampling**: repeat some training examples of the minority class.
- **Random undersampling**: retain fewer training examples of the majority class.
- **SMOTE**: impute synthetic minority-class examples based on neighbouring training examples.
- **Decision threshold**: select an operating point using validation data.

While we can apply resampling during training, we keep validation and test data untouched. During CV, resample separately inside each training fold using an `imbalanced-learn` pipeline.
### Feature Engineering: Creating New Features
**Feature engineering** is the process of creating, transforming, and selecting attributes from raw data to improve model performance. 

![[Pasted image 20261112171332.png|519]]
A model that is too simple for the data underfits. In this example, the linear model cannot properly represent the curve/turning point. We can use `PolynomialFeatures` to introduce a new feature:
```python
from sklearn.preprocessing import PolynomialFeatures

poly_features = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly_features.fit_transform(X)

# If X[0] is [-0.75275929], then X_poly[0] is [-0.75275929,  0.56664654
```

`PolynomialFeatures` multiplies inputs together to create new inputs, up to the max degree specified. If there are originally two features and degree=3, it would add $a^2, a^3, b^2, b^3, ab, a^2b,$ and $ab^2$. From two input features, we get 9, which can cause the model to overfit. Hence, we select the appropriate degree and use regularization only when needed.

![[Pasted image 20261112172621.png]]
We now get the new model $y=0.56x^2+(-1.06)x+1.78$. A model's linearity is in respect to it's coefficients (weights), not the input x. From the algorithm's point of view, it simply sees another column of numbers and learns a weight for it. `PolynomialFeatures` changes the feature representation, not the estimator.
### Data Quantity and Augmentation
Increasing the amount of training data samples can help improve robustness to variation that might be expected in deployment. However, it must be relevant, correctly labelled, and representative. This can be done in many different ways:
- **Images**: rotations, translations, crops, flips, noise, as long as these transformations preserve the label.
- **Text**: Paraphrasing/edits and language translations that are carefully controlled to preserve meaning and assigned label.
As with everything else, augmentation is applied only to training examples, and to each training fold during CV. 