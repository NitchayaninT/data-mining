# Classification Methods 2
- **Eager VS Lazy Learners**
	- Eager Learners 
		- Build model as soon as training data is available
		- Model is built ==before prediction==
		- Eg : Decision trees, rule-based classifiers
	- Lazy Learners
		- Don't build any model, only keep training data
		- Model is built ==during prediction==
		- Computation is done when predicting a new record
		- In KNN, prediction is based on class labels of nearest training records
- [Proximity Measures](#proximity-measures)
	- **Distance** between records
		- Between Nominal values
		- Between boolean values : Hamming
		- Between Numeric values : Manhattan, Euclidean, Chebyshev
	- **Similarity** between records
		- Between nominal values
		- Between boolean values : Simple matching coefficient (SMC), Jaccard 
		- Between numeric values : Cosine
- [K-Nearest Neighbors (KNN)](#k-nearest-neighbors-(KNN))
- [Naive Bayes](#naive-bayes)
	- Conditional Probability
	- Laplace correction
	
## Proximity Measures 
### Distance & Similarity between nominal values
- if 2 records are in the **same category** (same nominal value)
	- Distance = 0
	- **Similarity = 1**
- if 2 records are in **different category** (different nominal values)
	- **Distance = 1**
	- Similarity = 0
- Example : attribute color = {red,green,blue}
	- Record 1's color = red
	- Record 2's color = green
	- Record 3's color = red
		- Between records 1 and 2 : distance = 1, similarity = 0
		- Between records 1 and 3 : distance = 0, similarity = 1
		- Between records 2 and 3 : distance = 1, similarity = 0
### Distance & Similarity between boolean values
Example : There are 10 bits (boolean)
Record 1 = **1** 0 0 0 0 0 **0** 0 0 **0**
Record 2 = **0** 0 0 0 0 0 **1** 0 0 **1**
- **Distance : Hamming**

![hamming-distance.png](/pictures/hamming-distance.png)

- **Similarity : Simple Matching Coefficient (SMC)**

![SMC.png](/pictures/SMC.png)

Can be misleading because if the matching is 1,1 or 0,0, it cannot tell a difference. only that they're similar, it considers 0,0 (coabsence) to have equal weight as copresences, but in reality, sometimes coabsences isnt as meaningful.
If the categories (like is student, is not student) have equal weights, 1,1 and 0,0 both mean "same". So SMC is appropriate
- **Similarity : Jaccard**

![jaccard.png](/pictures/jaccard.png)

- co-presences (number of matching 1's)= 0
- co-absences (number of matching 0's) = 7
- total bits = 10
So, **jaccard = 0/(10-7)=0**
- Mutual absence doesnt necessarily imply similarity (like when both people not liking 90% of the movies, it doesnt make them similiar)
- So, jaccard gives unequal weight : focuses only on shared presences (1-1) and ignore shared co-absences
- if for Nominal attributes, its basically **#matches/#attributes**
- If for binomial attributes, its true = presence. false = absence
**Comparing SMC and Jaccard**
- Use SMC when we give ==equal weights== to both categories
- Use Jaccard when we give ==unequal weights== to both categories

### Distance between numeric values
Each record contains n numeric attributes, meaning there are n numbers (features) describing each record
- **Record X** = (x1,x2,...,xn)
- **Record Y** = (y1,y2,...,yn), where n = numeric attribute
- **Manhattan distance**

![manhattan.png](/pictures/manhattan.png)

	- Preferred over Euclidean distance when #attributes is large because euclidean distance (which sqaures and roots values) can be overly affected by one large difference
- **Euclidean distance**

![euclidean.png](/pictures/euclidean.png)

	- Most commonly used
	- Straight line distance between 2 points in n-dimensional space
	- Represents the true geometric distance between points
- **Chebyshev distance**

![Chebyshev.png](/pictures/Chebyshev.png)

	- Measures the largest single difference among all the attributes
	- It only cares about the biggest gap between the two records (which attribute has the biggest gap?)
These distances tell how **similar or different** the two records are in numeric terms

- **Minkowski Distance**
Manhattan, Euclidean, Chebyshev are special cases of Minkowski Distance

![Minkowski.png](/pictures/Minkowski.png)

![distance_all.png](/pictures/distance_all.png)

![distance_all2.png](/pictures/distance_all2.png)

- All points on **red line** have **Euclidean distance** = 2 from (0,0), circle has radius = 2, so all points on red circle are 2 units away from the middle
- All points on **blue line** have **Manhattan distance** = 2 from (0,0), the total steps horizontally and vertically add up to 2
- All points on **green line** have **Chebyshev distance** = 2 from (0,0), at least one coordinate on the square equals 2 (max distance between x1,x2 and y1,y2)

- **Mixed Euclidean** (for mixed types, including nominal and numeric)

![mixed-euclidean-formula.png](/pictures/mixed-euclidean-formula.png)

### Similarity between numeric values
- **Cosine Similarity** : Dot product between 2 vectors

![mixed-euclidean.png](/pictures/mixed-euclidean.png)


	- Completely similar = angle between vectors = 0
	- Completely dissimilar = angle between vectors = 90
	
![mixed-euclidean-example.png](/pictures/mixed-euclidean-example.png)

### Issues in Proximity Calculation
- a record consists of different types of attributes
- **Attributes have different scales**
	- Example : euclidean distance between people based on age and income attributes, the scales are different (distance is dominated by income)
- **Attributes have different weights**
	- Every attribute contribute unequally to the proximity between objects
	- For example, distance between students should be based on their GPA rather than height
	- **Fixed by attribute weighting** -> Multiplying GPA by a weight factor
	- Use **feature selection method** to determine attribute scores
## K-Nearest Neighbors (KNN)
- K = number of closest neighbors to look at
- **Odd K value is recommended** to avoid tie
- Each neighbor's vote can be weighted by the distance between the neighbor and the target
- KNN uses **Euclidean, Manhattan, or other distance metrics** to compute distance between the new point and all existing points (we can specify that by ourselves using python's sklearn)
- Pick the K smallest distances (nearest neighbors)
- This is mainly for **classification**, taking majority vote among the neighbors' labels (eg. if 2 of 3 neighbors are class A, predict A)
### Data normalization for KNN
- If numeric attributes have **different scales**, **normalize them** so that attributes with big scales don't dominate distance calculation
	- To normalize, use data preprocessing process like weight by gain ratio, chi square or manually and then "scale by weights" after that. 
	- After scaling, we can then split data for training and testing in KNN
### KNN Summary
- No learning phase (doesn't build a model, lazy learner)
- KNN simply stores the training data and waits until it needs to make a prediction
- So during training, it just memorizes the data. So "prediction" is expensive if training data is very big cuz we have to **compare target record with every training record** to find **K nearest neighbors**
- KNN predicts the class of a new sample based on its **neighbors** in the **training set**.
- K too small -> **sensitive to noise** in the data instead of general patterns, overfitting
- K is too big -> misclassification 
- KNN is also a popular method to **impute missing values**
## Naive Bayes
- It **predicts a class** based on how likely that class is **given** the feature values (how likely it is to see this attribute value GIVEN the class)
- Prediction is done by comparing P(Y=yes|X) and P(Y=no|X) and choose class label with higher probability

![naive_bayes.png](/pictures/naive_bayes.png)

- P(X) can be ignored
- But finding P(X|yes), P(X|no) is not easy because X is not a single value (it contains many attributes), we have to compute **P(A1,A2,...,An|yes)** and **P(A1,A2,...,An|no)**

### Naive assumption
- Assuming the **attributes are independent**

![naive-assumption.png](/pictures/naive-assumption.png)

- To compute each P(Ai=a|yes) from training data
	- **Nominal attr** : use relative frequency that Ai=a in class yes (eg, how often Color = red appears in Fruit class = apple)
	- **Numeric attr** : use probability density from a Normal distribution function. (eg, in class "Yes", the attribute "Age" might have a mean of 30 and stdev of 5, so most "Yes" people are around 30 years old)
	
![naive-numeric.png](/pictures/naive-numeric.png)

![naive-numeric2.png](/pictures/naive-numeric2.png)

		- a is the attribute from the value that we wanna predict class 'yes' or 'no'. we have to calculate mean and SD for both class yes and class no first
		- if the probability of class yes is higher than no, then it predicts yes

### example

![naive-example.png](/pictures/naive-example.png)

** the denominator must be the **count of rows in the conditioned class (yes, no),** not total dataset
**House**
- P(owner|yes) = 0/3
- P(tenant|yes) = 3/3
- P(owner|no) = 3/7
- P(tenant|no) = 4/7
**Marital Status**
- P(single|yes) = 2/3
- P(married|yes) = 0/3
- P(divorced|yes) = 1/3
- P(single|no) = 2/7
- P(married|no) = 4/7
- P(divorced|no) = 1/7
**Income**
- no -> mean = 110, var = 2975
- yes -> mean = 90, var = 25
### predicting a new record
- after we get the info of P(Ai|class), we can now predict which class the new value belongs to

![naive-example2.png](/pictures/naive-example2.png)

- **Prediction = No**

### Laplace Correction
- Zero probability causes the whole p(yes|X) to be zero
- To avoid this, we **add 1 to the frequency of each category** if attribute A has k categories

![laplace-correction.png](/pictures/laplace-correction.png)

![laplace-example.png](/pictures/laplace-example.png)

- + 2 at the denominator because **attribute has 2 categories**, which are owner and tenant
### Predicting a new record (+Laplace correction)

![naive-with-laplace.png](/pictures/naive-with-laplace.png)

P(yes|X) + P(no|X) = 0.000432 + 0.0014 = 0.001832 
P(yes|X) = 23.58%, P(no|X) = 76.42%
**Prediction = no**

- For nominal attribute, **unknown category is added** because of laplace correction. If unknown not in training data, probability becomes 0 and kills that class. So the output shows unknown with a small number for completeness
- this is for the model to be able to make predictions even if an **unseen value appears**
### Naive Bayes Summary
- **Naturally handle noises** and **missing values**
	- When estimating conditional probability, noises are averaged out & missing values are ignored
	- If a record has a missing value, **that attribute is simply excluded** from the probability product
	- Because naive bayes computes probabilities **over the entire dataset**, it tends to average out rather than dominate. So noisy points have little effect on the overall conditional probability
- **Irrelevant features don't matter much**
	- example, suppose shoe size has nothing to do with whether someone buys a phone
	- when we multiply these probabilities across attributes, the ratio between classes is nearly 1, so it cancels out -> doesn't affect the decision
- Important assumption = **attributes are independent of each other**
	- but that's not true in most real data (features often correlate)
	- but in practice, naive bayes still works surprisingly well because its estimating the "most likely class", **not the exact probabilities**
	- Difficult to prove in practice

