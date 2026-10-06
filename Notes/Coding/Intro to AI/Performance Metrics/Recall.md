Also called **sensitivity** or **true positive rate (TPR)**, recall measures the proportion of predicted positives to the total amount of true positives. In other words, out of all the true positives, how many did the model predict?
$$recall=\frac{TP}{TP+FN}$$
Recall is important when discovery is important:
- ex: Cancer screening for malignant tumours: false negatives (missing a case) is far more dangerous than false positives; a high recall means finding nearly all patients with cancer; false alarms are a minor inconvenience.
- ex: security (better to flag all suspicious activity than miss a real attack).
