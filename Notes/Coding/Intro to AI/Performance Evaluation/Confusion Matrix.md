A **confusion matrix** is a table that summarizes the performance of a classification algorithm. 
![[Pasted image 20261006145757.png]]
- **True positive (TP)**: The true label was positive and the model predicted positive.
- **False positive (FP)**: the true label was negative and the model predicted positive.
- **False negative (FN)**: the true label was positive and the model predicted negative.
- **True negative (TN)**: the true label was negative and the model predicted negative.

The diagonal elements represent the correct predictions (TPs and TNs). The off-diagonal elements correspond to incorrect predictions.

A confusion matrix provides a summary, but there exist more concise metrics.
### Binary Classification
![[Pasted image 20261006150324.png]]

### Multiclass Classification

![[Pasted image 20261006150319.png]]

For a multiclass problem we derive a [[Classification Tasks#One-vs-Rest (OvR)||one-vs-rest]] 