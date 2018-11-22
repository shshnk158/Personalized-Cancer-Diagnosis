# Personalized-Cancer-Diagnosis
Classify the given genetic variations/mutations based on evidence from text-based clinical literature

## Business problem to solve
Understanding the business problem to solve is very important, that lets us know the real world constraints.so lets discuss more about it, without business constraints we cant put our models into production.

When a patient seems to have cancer, we take a cancer tumor from the patient and we go through genetic sequencing of DNA.Once sequenced, cancer tumor can have thousands of genetic mutations.‘mutation’ means a small change in gene which causes cancer. Here every gene is assosiated with a variation.Now with the help od gene and variation we need to classify which class it belongs to. We have 9 classes in our dataset out of which only few belongs to cancer classes.

According to this source (https://www.kaggle.com/c/msk-redefining-cancer-treatment/discussion/35336#198462), these are steps to be done manually to classify,
1. A molecular pathologist selects a list of genetic variations of interest that he/she want to analyze
2. The molecular pathologist searches for evidence in the medical literature that somehow are relevant to the genetic variations of interest
3. Finally this molecular pathologist spends a huge amount of time analyzing the evidence related to each of the variations to classify them.
Here, steps 1 and 2 can be done, but doing step 3 takes a lot of time, so we reduce time consuming using a machine learning model.

Finally our problem statement is **__ classify genetic variations based on evidence from the text based clinical literature or research papers, which is multi-class classification problem __** because we have 9 classes.

## Business constraints

1. Interpretability of the algorithm is must because, at the end doctor or specialist should understand why this model predicted that class.
2. No low-latency requirement which means patient can wait few minutes for results. This tells us that we can use complex models.
3. Errors are very costly.
4. Probabilities of data point to each class needed because, if two classes have somewhat simillar probs,doctor may suggest patient to go with more tests.

#### Performance metrics
Multi class Log-Loss,confusion matrix(Log-Loss is chosen because it actually uses probability which is our business constraint)

**Machine Learning objective:** Predict the probability of each data point belonging to each of nine classes.

## Dataset
Data was provided by Data from Memorial Sloan Kettering Cancer Center (MSKCC) and it can be found on kaggle https://www.kaggle.com/c/msk-redefining-cancer-treatment/data.
We have two data files, one conatins the information about the genetic mutations and the other contains the clinical evidence (text) that human experts/pathologists use to classify the genetic mutations.Both these data files are have a common column called ID which is used to join these two files.

After reading the data,we preprocessed the data like removing stopwords ,converting to lower case and removing punctuations etc. This is what dataset looks like
```
	ID	Gene	Variation		Class		TEXT
	0	FAM58A	Truncating Mutations	1	cyclin dependent kinases cdks regulate variety...
	1	CBL	W802*			2	abstract background non small cell lung cancer...
	2	CBL	Q249E			2	abstract background non small cell lung cancer...
	3	CBL	N454D			3	recent evidence demonstrated acquired uniparen...
	4	CBL	L399V			4	oncogenic mutations monomeric casitas b lineag...
```

**Splitting the data :** As the data is not temporal in nature which means not changing with time we can split the data randomly for training ,cross validation and testing.
Then after splitting the data it is also found out that training and test data having almost same distributions and from distributions it is clear that data is imbalanced.

<p align="center">
  <img src="https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/ytr.png" width="350" title="Train Distibution">
</p>
<p align="center">
  <img src="https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/ycv.png" width="350" title="CV Distibutiot">
</p>
<p align="center">
  <img src="https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/yte.png" width="350" title="Test Distibutio">
</p>

## Random Model:
Creating a random model, because As we know that log-loss is ranging from 0 to infinite.So we first defined one random model so that if our ML model has log-loss less than our random model then we can consider our ML model is good.

## Univariate Analysis
Checking each feature whether its usefull for prediction of class, or else removing that feature from our dataset. We do this by predicting the class only with this feature, and comparethe results with random model.

1. Gene Feature
Gene is categorical feature from that we observed that there are 229 types of unique genes out of which top 50 genes nearly contribute 75 percent of data.
<p align="center">
  <img src="https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/gene.png" width="350" title="Test Distibutio">
</p>
Now we featured the gene into vector by one hot encoding, apply simple logistic regression model and compared results with random model, which inturn found gene feature is important one.

2. Variation Feature
Here variation is also a categorical feature and we observed that 1927 unique variations out of 2124 present in training data which means most of variations occurred once or twice. CDF of variations looks as follows:
<p align="center">
  <img src="https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/var.png" width="350" title="Test Distibutio">
</p>
Applied Logistic regression model and results are less than random model. so we use variation feature.

3. Text Feature.
Ofcourse, we need to use text feature because we need to compare every gene, variation with clinical text data. even though we put this across random model and results are good. we used one hot encoding BoW model to convert words to vectors.

## ML Models

### 1. Naive Bayes
We know that for text data NB model is a baseline model.Now we applied the training data to the model and used the CV data for finding best hyper-parameter (alpha). With the best alpha we fitted the model and test data is applied to the model and found out the log-loss value is 1.35.We also checked the probabilities of each class for each data, and also shown what features impacted our result.
In the end, theres a table comparing all the models I have put to test.

### 2. KNN
As we know that k-NN model is not interpretable, means it doesnt give probabilistic outputs,even though we tried out to check log loss. we used response coding instead of one hot encoding because of curse of dimensionality problem.Found best k using hyperparameter search and with best k got log loss 1.10.

### 3. Logistic regression
We also know that LR works well with high dimension data and it is also interpretable. So we did for both class-balanced(oversampling of lower class points) and unbalanced and applied the training data to the model and used the CV data for finding best hyper-parameter (lambda).
And the described results are shown at end with a neat table format.

### 4. Linear SVM
We used Linear SVM(with class balancing) because it is interpretable and works very well with high dimension data. RBF Kernel SVM is not interpretable so we not used it . Now we applied the training data to the model and used the CV data for finding best hyper-parameter (C)
With the best C we fitted the model and test data is applied to the model and found out the log-loss value is 1.22

### 5. Random forest
- **One hot encoding:** Normally DT works well with low-dimension data.It is also interpretable. By changing the no of base learners and max depth in Random Forest Classifier we found that best base learners=1000 and max depth=10.Then we fitted the model with best hyper-parameters and test data is applied to it and found out that log loss value is 1.17

- **Response coding** By changing the no of base learners and max depth in Random Forest Classifier we found that best base learners=100 and max depth=5.Then we fitted the model with best hyper-parameters and found that train logloss is 0.06,and CV log loss is 1.41 which says that model is overfitted even with best hyper-parameters.So we don’t use RF+Response Coding .

### 6. Stacking Classifier
We stacked three classifiers named LR,SVM,NB and LR as meta classifier.Now we applied the training data to the model and used the CV data for finding best hyper-parameter.With the best hyperparameter we fitted the model and test data is applied to the model and found out the log-loss value is 1.15


**At the end, I tried all these models with TfidfVectorizer and also listed log loss below,**

