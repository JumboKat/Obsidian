#AI
A **decision tree** is a rooted, hierarchical structure used for regression and classification tasks. Each node marks a decision that performs a binary test on a certain feature (Must fit in one of two categories, or must be above or below a threshold number). The trees structure is inferred (learned) from the training data. Each leaf produces a prediction: a class label/class probabilities for classification or a numerical value for regression. Decision trees are useful because they clearly show the rules learned by the model.
![[Pasted image 20260923001058.png]]
#### Classifying New Instances (Inference)
When classifying a new data point, start at the root node of the decision tree. As you go down the tree, answer each binary question until you reach a leaf node. The label associated with that leaf is the classification of the instance.

When solving a regression task, each leaf node stores a prediction value:![[Pasted image 20260923112812.png]]
Where N<sub>leaf</sub> is the number of training samples within that leaf, and y<sub>i</sub> are their target values.
### Decision Boundaries
A **decision boundary** partitions the feature space into regions corresponding to different class labels.
#### Simple Decision Boundaries
In this example, the decision boundary that delineates Gentoo vs. not Gentoo penguins can be represented by a line:
![[Pasted image 20260923113316.png]]
In this case, the line is **linearly separable**; two classes of data can be perfectly separated by a single linear boundary (a line in 2-D space or a hyperplane in higher dimensions). 

In this example, a decision boundary in the shape of a quadratic curve fits the data better.
![[Pasted image 20260923113717.png]]
#### Complex Decision Boundaries
A decision tree can lead to irregular and non-linear decision boundaries. Each decision determines an edge of the boundary.
![[Pasted image 20260923113823.png]]
### Constructing a Decision Tree
