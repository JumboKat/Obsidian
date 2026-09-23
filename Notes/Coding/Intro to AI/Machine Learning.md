#AI
### Why should a Computer Learn?
- Adaptability and continuous improvement: adapt to changing conditions in dynamic settings and stay current with new trends.
	- etc: self-driving cars adjusting to traffic and weather changes.
	- etc: spam filters evolving to counter new spam techniques.
- Enhanced performance and efficiency: learn to improve performance based on past experience or data. Can also save on costs as it can remove the need for manual updates.
- Complex problem solving and hidden pattern discovery: can handle problems that are too intricate for static, rule-based systems (i.e. image recognition). Can also uncover relationships in data not seen before.
- Personalization: can provide tailored outputs to users.
- Scalability: can efficiently work with large datasets.
- Innovation and research: can stimulate new ideas.

There are three main types of machine learning:
1. **Unsupervised Learning**: data examples have no target labels (AI does not see the answer).
2. **Supervised Learning**: training examples have labels.
3. **Reinforcement Learning**: AI agent is rewarded for its actions to encourage it toward an objective.
### Unsupervised Learning
Since there are no labels in unsupervised learning, the AI tries to find patterns to establish some useful structure to them. 

Unsupervised learning tasks include:
- **Clustering** groups similar examples according to selected features. The groups are inferred and should not be treated as objectively correct or meaningful categories.
- **Dimensionality Reduction**: represents the data using fewer variables (dimensions) while still preserving the overall structure and patterns.
- **Density Estimation**
- **Anomaly Detection**: identifies things that differ from the expected pattern.
### Reinforcement Learning
In reinforcement learning, an agent performs actions in an environment and receives numerical rewards. The goal is for the AI to develop a policy that maximizes cumulative reward. This is done by repeating the process of making an observation, performing an action, and receiving a reward. Rewards aren't necessarily a target label specifying the correct action. They can also be delayed, forcing the agent to prioritize long-term gains. The main struggle of the AI is balancing exploration of unfamiliar actions and exploiting known actions that have proven effective.

Typical applications include games, robotics, resource allocation and control systems.
### Supervised Learning
The supervised learning workflow is divided into two phases: the **learning** phase (building and training the model) and **inference** (using the model). These phases may not always be distinct; many workflows (such as reinforcement learning) can be cyclical, with acting, receiving feedback, and making improvements being interleaved with one another.
#### Learning
![[Pasted image 20260919161631.png]]
Training a model can be the most challenging and resource-intensive part. Data must be curated, and an algorithm that best fits the data must be selected and trained. 

From raw training data, features are extracted, and each piece of data corresponds to a features vector. 
#### Inference
Inference is typically cheaper than learning, but the total cost can be substantial if the model serves many users, and some reasoning systems use more computation during inference.
### Example: Supervised Learning
Objective: develop a predictive model that classifies a fishing day as poor, average, or excellent.

Training a model on labelled data to make predictions on new data is called **classification**. "Poor," "Average," and "Excellent" are classes for the target variable.
#### Attributes
The following are the features that will be analyzed from the data to observe patterns and relationships:
- **Moon Phase (categorical)**: 'New Moon,' 'First Quarter,' 'Full Moon,' and 'Last Quarter.'
- **Forecast (categorical)**: 'Rainy,' 'Cloudy,' and 'Sunny.'
- **Outdoor Temperature (Numerical)**: The air temperature in degrees Celsius.
- **Water Temperature (Numerical)**: The water temperature in degrees Celsius.
#### Training Data
![[Pasted image 20260919163519.png]]
Since the target is known for each example, this is a **supervised-learning** problem, with the possible targets being categories making this a **classification task**.

From here, training data is split into data and labels. The data is usually presented in tabular (matrix) format where each row represents a feature vector denoted as x_i, corresponding to the i-th example in the training set. The labels are represented as a column vector, with y_i denoting the i-th label example.
#### Model Training
This step involves using labelled examples to construct a model that can make predictions. In this case, a model that could arise would be:
- If the forecast is Sunny, predict Excellent
- If the forecast is Cloudy, predict Average
- If the forecast is Rainy, predict Poor
Fitting the model to the example data does not guarantee that it will be able to make accurate predictions on new data.
### Life Cycle
The life cycle of a machine learning model is as follows:
1. **Data collection and preparation**: data should be sufficient, high quality (low noise), few or no missing values, and representative. 
2. **Feature engineering**: selecting, transforming, and creating input variables (features) that help a model learn useful patterns. This can include scaling numerical variables, representing categorical variables, and constructing new features.
3. **Training**
4. **Model evaluation**: assessing the model's performance based on metrics such as accuracy, precision, recall, F1-score, and/or AUC-ROC. Usually, the model is tested on a separate validation/test dataset or split of the dataset to ensure it generalizes well to new data and not simply memorizing the training data.
5. **Model deployment**: integrating the model into an application. Many deployed models have fixed parameters and are periodically updated, while others continuously learn from new data. 
6. **Monitoring and maintenance**: model performance is continuously monitored. *Concept drift* is often observed (the data the model was trained on may become obsolete or outdated), requiring the system to be retrained.
### Formal Definitions
#### Supervised Learning
The **dataset** is a collection of labelled examples.
![[Pasted image 20260923000122.png]]

A **classification** task is when the target y<sub>i</sub> is a discrete class, among a finite set of classes. When the target is a real number, it is a **regression** task.