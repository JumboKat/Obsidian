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
