# Azure AI/Fundamentals of Machine Learning

https://learn.microsoft.com/en-us/training/modules/fundamentals-machine-learning/1-introduction

### Supervised Machine Learning 
: Training data includes both *feature* values and known *label* values. Trains models by determining a relationship between the features and labels in past obversations. 

* **Regression** - a form of supervised maching learning in which the label predicted by the model is a numeric value.
* **Classification** - form of supervised machine learning in which the label represents a categorization, or class. There are two common classification scenarios.
  * **Binary Classification** -  the label determines whether the observed item is (or isn't) an instance of a specific class
    * i.e. patient is at risk of diabetes
    * bank custill will or won't default on a loan
  * **Multiclass Classification** - extends binary classification to predict a label that represents one of multiple possible classes.
    * Determine species of penguin based on physical measurements
    * Determine the genre of a moview based on case, director, budget.

### Unsupervised Machine Learning 
*Unsupervised* machine learning involves training models using data that consists only of feature values without any known labels. Unsupervised machine learning algorithms determine relationships between the features of the observations in the training data.
* **Clustering** - A clustering algorithm identifies similarities between observations based on their features, and groups them into discrete clusters
  * group similar flowers based on size, number of leaves/petals
  * identify groups of similar customers based on demographic attributes and purchasing behavior

In some cases, clustering is used to determine the set of classes that exist before training a classification model. For example, you might use clustering to segment your customers into groups, and then analyze those groups to identify and categorize different classes of customer (high value - low volume, frequent small purchaser, and so on).

### Regression models
 Example Steps:
 1. Split the training data (randomly) to create a dataset with which to train the model while holding back a subset of the data that you'll use to validate the trained model.
2. Use an algorithm to fit the training data to a model. In the case of a regression model, use a regression algorithm such as linear regression.
3. Use the validation data you held back to test the model by predicting labels for the features.
4. Compare the known actual labels in the validation dataset to the labels that the model predicted. Then aggregate the differences between the predicted and actual label values to calculate a metric that indicates how accurately the model predicted for the validation data.

#### Regression evaluation metrics
Mean Absolute Error (MAE)
: Mean of the Absolute error for each prediction (i.e. middle)

Mean Squared Error (MSE)
: it may be more desirable to have a model that is consistently wrong by a small amount than one that makes fewer, but larger errors. One way to produce a metric that "amplifies" larger errors by squaring the individual errors and calculating the mean of the squared values. This metric is known as the mean squared error (MSE).

Root Mean Squared Error (RMSE)
: To measure the error in terms of the number of units, we need to calculate the square root of the MSE.

Coefficient of determination (R2)
: Compares the sum of squared differences between predicted and actual labels with the sum of squared differences between the actual label values and the mean of actual label values. Result is always a decimal between 0 and 1. The closer to 1 this value is, the better the model is fitting the validation data.

Iterative training
: In most real-world scenarios, a data scientist will use an iterative process to repeatedly train and evaluate a model, varying:
* Feature selection and preparation (choosing which features to include in the model, and calculations applied to them to help ensure a better fit).
* Algorithm selection (We explored linear regression in the previous example, but there are many other regression algorithms)
* Algorithm parameters (numeric settings to control algorithm behavior, more accurately called hyperparameters to differentiate them from the x and y parameters).

### Binary Classification

Instead of calculating numeric values like a regression model, the algorithms used to train classification models calculate probability values for class assignment and the evaluation metrics used to assess model performance compare the predicted classes to the actual classes. 
*Binary classification* algorithms are used to train a model that predicts one of two possible labels for a single class. i.e. true or false, 1 or 0, etc.

Probability is measured as a value between 0.0 and 1.0, such that the total probability for all possible classes is 1.0. So for example, if the probability of a true is 0.7, then there's a corresponding probability of 0.3 of a false.

#### Binary classification evaluation metrics
Confusion Matrix - 4 square with 
True Negatives in top left, False Positives in top right
False Negatives in bottom left, True Positives in bottom right

Accuracy
: the proportion of predictions that the model got right. Accuracy is calculated as:
(TN+TP) ÷ (TN+FN+FP+TP)

Recall 
: metric that measures the proportion of positive cases that the model identified correctly. In other words, compared to the number of patients who have diabetes, how many did the model predict to have diabetes?
The formula for recall is: TP ÷ (TP+FN)

Precision
: similar metric to recall, but measures the proportion of predicted positive cases where the true label is actually positive. In other words, what proportion of the patients predicted by the model to have diabetes actually have diabetes?
The formula for precision is: TP ÷ (TP+FP)

F1-score
: overall metric that combined recall and precision. The formula for F1-score is:
(2 x Precision x Recall) ÷ (Precision + Recall)

Area Under the Curve (AUC)
: a received operator characteristic (ROC) curve that compares the TPR and FPR for every possible threshold value between 0.0 and 1.0. If the Area Under the Curve is > .5, then it is better than random guessing.

### Multiclass Classification
*Multiclass classification* is used to predict to which of multiple possible classes an observation belongs. As a supervised machine learning technique, it follows the same iterative train, validate, and evaluate process as regression and binary classification in which a subset of the training data is held back to validate the trained model.

One-vs-Rest algorithms 
: train a binary classification function for each class, each calculating the probability that the observation is an example of the target class. Each function calculates the probability of the observation being a specific class compared to any other class

Multinomial algorithms
: As an alternative approach is to use a multinomial algorithm, which creates a single function that returns a multi-valued output. The output is a vector (an array of values) that contains the probability distribution for all possible classes - with a probability score for each class which when totaled add up to 1.0
[0.2, 0.3, 0.5]
The elements in the vector represent the probabilities for classes 0, 1, and 2 respectively; so in this case, the class with the highest probability is 2.

### Clustering
Clustering is a form of unsupervised machine learning in which observations are grouped into clusters based on similarities in their data values, or features. This kind of machine learning is considered unsupervised because it doesn't make use of previously known label values to train a model. In a clustering model, the label is the cluster to which the observation is assigned, based only on its features.

K-Means clustering algorithm
: 1. The feature (x) values are vectorized to define n-dimensional coordinates (where n is the number of features). In the flower example, we have two features: number of leaves (x1) and number of petals (x2). So, the feature vector has two coordinates that we can use to conceptually plot the data points in two-dimensional space ([x1,x2])
  1. You decide how many clusters you want to use to group the flowers - call this value k. For example, to create three clusters, you would use a k value of 3. Then k points are plotted at random coordinates. These points become the center points for each cluster, so they're called centroids.
  2. Each data point (in this case a flower) is assigned to its nearest centroid.
  3. Each centroid is moved to the center of the data points assigned to it based on the mean distance between the points.
  4. After the centroid is moved, the data points may now be closer to a different centroid, so the data points are reassigned to clusters based on the new closest centroid.
  5. The centroid movement and cluster reallocation steps are repeated until the clusters become stable or a predetermined maximum number of iterations is reached.

Metrics used to evaluate a clustering model.
* **Average distance to cluster center**: How close, on average, each point in the cluster is to the centroid of the cluster.
* **Average distance to other center**: How close, on average, each point in the cluster is to the centroid of all other clusters.
* **Maximum distance to cluster center**: The furthest distance between a point in the cluster and its centroid.
* **Silhouette**: A value between -1 and 1 that summarizes the ratio of distance between points in the same cluster and points in different clusters (The closer to 1, the better the cluster separation).

### Deep Learning
*Deep learning* is an advanced form of machine learning that tries to emulate the way the human brain learns. The key to deep learning is the creation of an artificial *neural network* that simulates electrochemical activity in biological neurons by using mathematical functions

### Azure Machine Learning
Cloud service for training, deploying, and managing machine learning models. It's designed to be used by data scientists, software engineers, devops professionals, and others to manage the end-to-end lifecycle of machine learning projects, including:

* Exploring data and preparing it for modeling.
* Training and evaluating machine learning models.
* Registering and managing trained models.
* Deploying trained models for use by applications and services.
* Reviewing and applying responsible AI principles and practices.

Requires a *Azure Machine Learning workspace*. The workspace can then be used in *Azure Machine Learning Studio* which can:
  * A job must be created in Machine Learning studio to use Machine Learning to train a regression model. A workspace must be created before you can access Machine Learning studio.
* Import and explore data.
* Create and use compute resources.
* Run code in notebooks.
* Use visual tools to create jobs and pipelines.
* Use automated machine learning to train models.
* View details of trained models, including evaluation metrics, responsible AI information, and training parameters.
* Deploy trained models for on-request and batch inferencing.
* Import and manage models from a comprehensive model catalog.



**Multiple linear regression** models a relationship between two or more features and a single label. 
  * Multiple linear regression models the relationship between several features and a single label. The features must be independent of each other, otherwise, the model's predictions will be misleading.
  **Linear regression** uses a single feature. 

**Logistic regression** is a type of classification model, which returns either a Boolean value or a categorical decision. 
**Hierarchical clustering** groups data points that have similar characteristics.

The **regression algorithms** are used to predict numeric values. 
**Clustering algorithms** groups data points that have similar characteristics
**Classification algorithms** are used to predict the category to which an input value belongs. 
**Unsupervised learning** is a category of learning algorithms that includes clustering, but not regression or classification.
