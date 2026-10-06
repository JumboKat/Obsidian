For multiclass problems, micro and macro averaging is used to combine per-class results into one number. With multiple classes, we compute [[Classification Tasks#One-vs-Rest (OvR)]||one-vs-rest]] metrics on each individual class, treating it as the positive and the rest as the negative. This gives a precision, recall, and F1 score per class. Averaging then takes all of these and collapses them into one score.
### Macro Averaging
1. Compute metrics separately for each class.
2. Take the plain average of these metrics

Every class has equal weight, regardless of their representation. One minority class can swing the average noticeably in either direction.

### Micro Averaging
1. Add up the TP, FP, and FN across all classes.
2. Compute the metrics once from those totals.
Every prediction counts equally, so majority classes can dominate the result.

Micro works at the level of individual predictions (pool everything, compute once), while macro works at the level of classes (compute per class, then average).

Micro is a better choice if you care about the overall performance across all examples, while macro is a better choice if there is a class imbalance and you want to give rare classes importance.