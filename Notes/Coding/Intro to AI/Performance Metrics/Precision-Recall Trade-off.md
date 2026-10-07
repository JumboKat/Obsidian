Some classifiers like SGDClassifier, produce a linear score. We can delineate classes by establishing a threshold (for [[Logistic Regression||logistic regression]], the default is 0.5). Where we set the threshold determines the level of precision and recall:

![[Pasted image 20261006215443.png]]
- A low threshold means more predicted positives; higher recall and lower precision
- A high threshold means fewer predicted positives; lower recall and higher precision

The precision-recall curve plots precision vs. recall for each possible threshold:
![[Pasted image 20261006220103.png]]
![[Pasted image 20261006220249.png|410]]
