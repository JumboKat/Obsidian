The **Receiver Operating Characteristic (ROC) curve** plots **true positive rate (TPR)** against **false positive rate (FPR)**. An ideal classifier has TPR close to 1 and FPR close to 0.
- TPR = $\frac{TP}{TP+FN}$ (recall/sensitivity; how many out of the total positives were predicted)
- FPR = $\frac{FP}{FP+TN}$ 
- TPR approaches 1, FPR approaches 0 when there are few false negatives
- At the extreme threshold = 0, everything is predicted positive (1,1).
- At the extreme threshold = 1, everything is predicted negative (0,0).
![[Pasted image 20261006221511.png]]

The ROC curve summarizes the score ranking across thresholds; a decision picks one point in particular based on the relative costs of false positives and false negatives. We can apply the ROC curve to multiclass problems using one-vs-rest.

An ROC curve can be plotted by hand by:
- Taking the sorted unique scores (plus infinity) as thresholds
- For each threshold, predict positive if score >= threshold.
- Compute TP, FN, FP, TN, then TPR and FPR.
- Plot the (TPR, FPR) pairs
### Area Under the ROC Curve (AUROC)
The **AUROC** summarizes how well a score ranks positive examples above negative examples.
- AUROC = 1: every positive example receives a higher score than every negative example; the two groups can be cleanly separated.![[Pasted image 20261006222026.png]]

- AUROC = 0.5: random ranking on average.
![[Pasted image 20261006222048.png]]

- AUROC < 0.5: ranking is systematically reversed (scoring is inversed)
![[Pasted image 20261006222059.png]]

AUROC does not measure probability calibration or select a threshold. Moving a threshold changes predicted labels, confusion matrix, accuracy, and the ROC operating point, but not AUROC, since score ordering is unchanged.