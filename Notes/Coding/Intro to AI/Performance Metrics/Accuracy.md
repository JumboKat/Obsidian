**Accuracy** measures how accurate the result is, or the ratio of correctly predicted instances to the total number of predictions. In other words, what proportion of predictions were correct?

Accuracy is calculated as:$$accuracy=\frac{TP+TN}{TP+TN+FP+FN}=\frac{TP+TN}{N}$$
- Where N is the number of predictions.

As a ratio, it is a number between 0 (all wrong) and 1 (perfect).
### Class Imbalance
Accuracy alone can be misleading due to class imbalance. If one class contains substantially more examples than another, accuracy can seem high if the model excels at predicting the majority class but performs poorly on the minority class, something that may be hidden by the accuracy ratio.

