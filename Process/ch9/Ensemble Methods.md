# Ensemble Methods
- **Individual vs Ensemble classifiers**
- **Ensemble methods**
	- Voting, Stacking
	- Bagging, random forest
	- Boosting (Adaboost), Gradient boosting tree
	- Multi-class to binary classification
## Individual VS Ensemble Classifiers
- Ensemble Classifier : it is **many models working together** to make a final prediction.
- In ensemble model, prediction is based on **majority vote.** (the class that most models predict). Errors occurs if more than the majority makes wrong choice
- **A weak classifier = one small/simple model (base classifier)**  
- **A strong classifier = many weak classifiers combined (ensemble classifier)**
![[Pasted image 20251113230938.png]]

This graph shows the error of the ensemble when we combine many base classifers (eg. Majority voting)
- If the **base classifer is better than random guessing** (error < 0.5), ensemble error **drops down to 0** (red line)
- If the **base classifier is worse than random** (error > 0.5), ensemble becomes very bad, shoots up toward 1
### When is ensemble classifier better than individual one?
- Each **base classifier does better than random guessing** (error < 0.5)
- Base classifier is independent of each other (difficult to prove)
- Base classifier is **sensitive to minor changes** in training data
	- Any little change may result in completely different model
	- **Reason** : data set is too small, contain noises, much variation in data
- **Too many attributes** for just 1 classifier

![[Pasted image 20251113232551.png]]
- **Bias = Shooting angle**. Error due to **wrong strong assumptions**
	- Distance from the actual target
	- Classifier underfits the data, fails to learn pattern (high bias)
	- Classifier with many parameter setting tends to have more bias
	- Can be controlled by parameter adjustment
- **Variance = Shooting force**. **How much** the model’s predictions change **if the training data changes.**
	- **Variation of the hits**
	- Effect of training data
	- Classifier that is sensitive to input data (overfitting) tends to have **more variance** because it relies heavily on the exact details of the training data. So **if we do smth with the model like remove a few points, it will change drastically**
	- Model : can bend and twist to fit many patterns (also fit noise), so it depends "heavily" on the exact training data, but performs poorly on testing data
- **Noise = Observed target location**
	- Effect of intrinsic noise in target classes

![[Pasted image 20251113233507.png]]

### How to ensemble base classifiers?
- **Use different types (or parameters) of base classifiers**
	- Train base classifiers **by the same training data**
	- They **learn different patterns** (different algorithms, like SVM, neural net, K-Nearest Neighbors, etc)
	- Ensemble methods -> ==voting, stacking==
- **Use different training records**
	- Generate different training sets by ***sampling with replacement***
	- Train base classifiers **by different training data**
	- Ensemble methods -> ==bagging, boosting==
- **Use different sets of attributes**
	- **Generate different training sets** by sampling records with replacement and sampling attributes
	- Train base classifers **by different training data**
	- Ensemble methods -> ==random forest==
- **Use different class labels**
	- Convert multi-class problem to 2-class problem
	- Train multiple binary classifiers
	- Ensemble methods -> ==1 against 1, 1 against all==
- **Ensemble methods involve training many base classifiers**
	- Simple classifiers : Decision stump, OneR
## Ensemble Methods
- [Voting](#voting) : Majority vote : **>= 1 base classifiers**
- [Stacking](#stacking) : **>= 1 base classifiers**
- [Bagging](#bagging) : **1 base classifier**
- [Boosting (Adaboost)](#boosting) : **1 base classifier**
- [Multi-class handling](#multi-class-handling): 1-against-1, 1-against-all

### Voting
Train & test different classifiers (recommended to be odd), get performance
Final prediction is based on majority vote
- Every model has the same weights, it just uses majority vote for each prediction
### Stacking
Splits data into 2 partitions
**Stacking** tries to **learn how to combine multiple models** (base classifiers) in a smart way
- **Base training**
	- Use **training partition (1)** to train base classifiers
- **Meta training**
	- Let each base classifier predict training partition (2)
	- Use **prediction results by base classifiers** to train meta classifier
	- Meta classifier learns **prediction patterns of base classifiers that lead to the correct prediction**
	- Simple classifiers are often used as meta classifiers
	- each model has different weights
After this, **apply base models** to predict on **test data**
These predictions -> **meta-features**
Then use **meta classifiers** to predict final output

**Example** : we have 3 base models (trained by Decision Tree, SVM,kNN). we trained EACH of them normally using partition (1).
Another partition (2) is used for meta training, it uses the **prediction results from base classifiers** + **true label from partition (2)**.
- It uses decision tree or rule induction as meta classifier
- meta classifier **contain attributes that are prediction results from base classifiers**
Instead of learning from original features, it learns from **which base model to trust** in which situation.
![[Pasted image 20251206160234.png]]
- Those confidence values **come directly from each base classifier**
Rule model example : **if base_confidence_yes0 <= 0.229, then predict no**
### Bagging
Manipulate training data, training n base classifiers of the **same type**
For each **base classifier training**
**bootstraping** = used to create diverse samples,  it generates different subsets of the training data set.
1. Create training data or bootstrap by **sampling with replacement** (at random)
2. Use training data to train base classifier
**Final prediction** : by majority vote (or avg prediction confidence) from n base classifiers
**Sampling with replacement** : 
- **Some records are chosen many times** while some are never chosen (cuz bootstrap sampling is random)
- On average, each bootstrap contains about 63% of original data (according to probability calculation)

**Example** **Bagging** : Given original data
Sampling with replacement : some records are chosen at many times/some not at all. It uses the same classifier type, but not with the same training data
![[Pasted image 20251207140158.png]]
![[Pasted image 20251207140524.png]]

Prediction by base classifiers
- Final decision = **majority vote**
- **Combine the predictions** of 0.1-1 from all base classifiers and cast a vote. Vote + if these base classifiers predict more + than -, vice versa
- Then, find the **accuracy** : compare the voting results with actual data, how many did our voting predicted correctly? (**correct/no. of data**)
### Boosting
---
- Training **n base classifiers** of the same type
- Let all training records **have equal weights** (initial state)
**For each base classifier training** {
1. Create training data by ***weighted sampling with replacement***
2. Use training data to **train base classifier**
3. Use base classifier to classify all training records
	- Calculate ***weight of this classifier (alpha)***. Low error = high alpha
		- So classifiers with **higher α** influence the final decision more
	- Update ***weight of training records***. Correctly predicted records get lower weights, misclassified points get higher weights
		- **Why?** So that the _next classifier_ pays more attention to the “hard” cases (boosts weights, Next classifier must try harder to get label correctly, **fix the mistakes** of earlier ones). This make the records with high weight **APPEAR more often** in later rounds
}
- Final prediction = ***weighted vote (by alpha)*** from n base classifiers

***Weighted sampling with replacement***
- Records with higher weights (the ones that get wrong prediction) will be sampled more often
- The next base classifiers focus more on hard-to-predict records
- But **it doesnt mean later classifiers are better** overall because they may be too focused on certain records, **so the prediction from each classifier is weighted by α**
### Example Boosting
---
**Given the same original data**
![[Pasted image 20251114002213.png]]
- Weight of **each training record w**
	- **First round** : w = 1/n (n = # training records), all records have same weight
	- **Next round** : ![[Pasted image 20251114002344.png]]
		- **epsilon** = the **sum of weights** of the **training samples** that the classifier **predicts WRONG** ***
	- w_new is normalized by their sum, so that **sum of w_new from all records = 1**
	- **Classifier error** in each round ![[Pasted image 20251114003001.png]]
	- Weight of **each base classifier** ![[Pasted image 20251114003015.png]]
		- **Higher error (epsilon)** = **lower weight** or creditability **for base classifier**

***Boosting Round 1***
![[Pasted image 20251114003841.png]]
![[Pasted image 20251114003846.png]]

***Boosting Round 2***
![[Pasted image 20251114003915.png]]
![[Pasted image 20251114003923.png]]

***Boosting Round 3***
![[Pasted image 20251114003947.png]]
**Adjust weight for next round**
- epsilon = 0.047 + 0.047 + 0.047 + 0.047 = **0.188**
- alpha = 0.5 log (0.812/0.188) = **0.7315**

![[Pasted image 20251114004256.png]]

**x = 0.1** -> y = 0.4236-0.6625+0.7315 -> **class +**
**x = 0.4** -> y = -0.4236-0.6625+0.7315 -> **class -**
**x = 0.8** -> y = -0.4236+0.6625+0.7315 -> **class +**

### Multi-class handling
From multi-class -> binary classification
**Why use binary instead** ? : some methods only support binary classification (SVM, Logistic regress), class imbalance (some class has more members), classifying too many classes can lead to poor accuracy
- **1-against-1**
- **1-against-all**
#### 1-against-1
Train binary classifier to distinguish **between each pair of classes**
C1-C2, C1-C3, C1-C4, C2-C3, C2-C4, C3-C4 (all possible combinations)
- when training each binary classifier, irrelevant records from other classes are ignored
- Then, let **all binary classifiers predict a new record**
	- Eg : Prediction = (C1,C1,C1,C3,C2,C3)
	- Using majority vote, final pred = C1
	- or compare avg prediction confidence of C1,C2,C3,C4 and choose the one with **highest confidence**
#### 1-against-all
Train binary classifier to predict **whether records belong to a class or the others**
- C1-others, C2-others, C3-others, C4-others
- Then, let all binary classifier predict a new record
- Eg : **predictions = (C1, others, others, others)**
![[Pasted image 20251207144959.png]]
- Using majority vote, final prediction = C1, or choose class with highest prediction confidence
### Comparison
In many cases, their performance are similar
- One against one **requires n(n-1)/2 classifiers** for n classes
	- Training is more computationally expensive
	- More robust to class imbalance (we consider only 2 classes at a time)
- One against all **requires only n classifiers for n classes**
	- **Faster training** (especially when n is big)-> more popular
	- But more sensitive to class imbalance problem -> when classifying a very small class against all the others 
## Summary
- Voting & Stacking : supports different classifiers
- Bagging & Boosting : are useful when the classification is susceptible to **data variation**
	- Base classifiers are trained from **different subsets of data** (different focus yields different bias) -> data variation is **neutralized by ensemble decision**
	- Boosting can handle hard-to-predict data & thus correct mistakes by previous base classifiers. but the model is more likely to be overfitting