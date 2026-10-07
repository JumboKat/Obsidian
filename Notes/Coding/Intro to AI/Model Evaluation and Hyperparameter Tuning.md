### Cross-Validation
Cross-validation is a method used to evaluate the performance of a model by splitting the dataset into training subsets and a testing subsets. 
#### The Holdout Method
This is the simplest form of splitting, creating one training subset and one testing subset (usually 80-20).
##### Considerations for Choosing Split Ratio
1. Dataset size: there should be enough samples in the test subset to train on. With a larger dataset, we can dedicate a greater proportion of samples to the training split.
2. Model complexity: