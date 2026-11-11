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

Ensuring consistency in encoding across training, validation, and production datasets is essential.

### ColumnTRa