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

After reading the data,we preprocessed the data like removing stopwords ,converting to lower case and removing punctuations etc

**Splitting the data :** As the data is not temporal in nature which means not changing with time we can split the data randomly for training ,cross validation and testing.
Then after splitting the data it is also found out that training and test data having almost same distributions and from distributions it is clear that data is imbalanced.

![alt text](https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/ytr.png) ![alt text](https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/ytr.png) ![alt text](https://github.com/shshnk158/Personalized-Cancer-Diagnosis/blob/master/Images/ycv.png)


