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
### Entropy
A **pure node** contains examples from only a single class, while a **mixed node** contains examples from several classes. A split is useful if it produces children that are *less* mixed than their parent.

**Entropy** is a numerical score that allows for candidate splits to be compared. Pure nodes have a score of zero, and the score increases as the proportions of each class become more evenly balanced. The entropy of a node i is calculated as:
$$H_i=-\sum_{k=1}^{K}p_{i,k}log_2p_{i,k}$$
Where:
- p<sub>i,k</sub> be the proportion of examples from class k.
- H<sub>i</sub> = 0 when the node is pure.
-  H<sub>i</sub> increases as the classes become more evenly represented.
- A smaller value is better.
#### Binary Entropy
If p is the proportion of one class (e.g. Gentoo penguins), then 
$$H(p)=-plog_2p-(1-p)log_2(1-p)$$
![[Pasted image 20260923121049.png]]
Entropy is greatest when the two classes are equally likely. At p = 0 or p = 1, the class is certain (only one is represented) and entropy is zero. 
### Evaluating a Split
A split using feature j and threshold t produces two children, those who fall below the threshold, and those who are above it. The split's score is calculated by its **weighted entropy**:
$$J(j,t)=\frac{N_{left}}{N_{parent}}H_{left}+\frac{N_{right}}{N_{parent}}H_{right}$$
We want to choose a feature and threshold that minimize the weighted entropy J(j,t). 

Weights matter; a single pure example should not outweigh leaving almost all of the rest of the examples in a highly mixed child. Each child's weight is proportional to the number of examples it contains. Because the parent is the same for all candidate splits, we can directly compare the weighted entropy of their children. Below is an example:

![[Pasted image 20260924100055.png]]
The first split leaves both children nearly as uncertain as the parent. The second split isolates a pure node, but it contains one example, and results in a very high entropy in the right child.

![[Pasted image 20260924100319.png]]
Accounting for their weights shows us that the isolated one, while it may appear best, doesn't actually show the overall entropy. The third candidate is actually the best split.
### Greedy Split Search
**Greedy Split Search** is the core algorithm used by decision trees to make splits using the following steps:
1. For every feature, generate its candidate thresholds.
2. For every threshold, divide the examples into left and right children.
3. Reject splits that create children smaller than *min_samples_leaf*.
4. Compute the weighted entropy.
5. Retain the feature and threshold with the smallest score.

This approach is called "greedy" because it simply picks whatever split gives the best score right now, not accounting for splits down the tree. It's fast and simple, but not guaranteed to build the best tree.
### Stopping Criteria
We stop making splits when one of the criteria are met, some of which include:
- All examples in a given node belong to the same class.
- The *max_depth* of the tree has been reached.
- The node would have less than *min_samples_split*.
- None of the candidate splits decreases entropy (less than *min_impurity_decrease*).
### Limitations
- Possible to create very large trees, which can be hard to interpret and are too specific/leads to overfitting.
- Greedy algorithm does not guarantee the optimal tree.
- Small changes in the data set can produce vastly different trees.