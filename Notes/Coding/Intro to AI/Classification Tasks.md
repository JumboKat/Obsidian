Classification tasks involves categorizing an example into one or more discrete **classes**. 

### Binary Classifcation
**Binary classification** is a supervised learning task where the objective is to categorize examples into one of two discrete classes. [[Logistic Regression||Logistic regression]] and **support vector machines (SVMs)** are such examples.

### Multiclass Classification
**Multiclass classification** categorizes examples into one of three or more classes. This is not to be confused with **multilabel classification**, where examples can belong to multiple classes. 

#### One-vs-Rest (OvR)
A multiclass classification problem can be approached as a collection of binary classification tasks. in **OvR**, a separate binary classifier is trained for each class, which tries to classify the chosen class against all other classes. Afterward, for each example, the class whose classifier scores the highest is selected.