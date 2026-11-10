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
The method of encoding numerical features should match the data's meaning. It should preserve whether ca