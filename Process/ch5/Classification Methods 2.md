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

![[Screenshot 2025-10-17 at 12.17.10.png]]

![[Screenshot 2025-10-17 at 12.16.19.png]]

![[Screenshot 2025-10-17 at 12.19.24.png]]

- All points on **red line** have **Euclidean distance** = 2 from (0,0), circle has radius = 2, so all points on red circle are 2 units away from the middle
- All points on **blue line** have **Manhattan distance** = 2 from (0,0), the total steps horizontally and vertically add up to 2
- All points on **green line** have **Chebyshev distance** = 2 from (0,0), at least one coordinate on the square equals 2 (max distance between x1,x2 and y1,y2)

- **Mixed Euclidean** (for mixed types, including nominal and numeric)

![[Screenshot 2025-10-17 at 14.20.50.png]]

### Similarity between numeric values
- **Cosine Similarity** : Dot product between 2 vectors

![[Screenshot 2025-10-17 at 12.47.40.png]]


	- Completely similar = angle between vectors = 0
	- Completely dissimilar = angle between vectors = 90
	
![[Screenshot 2025-10-17 at 12.48.49.png]]

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
- If numeric attributes have different scales, normalize them so that attributes with big scales don't dominate distance calculation
- K = number of closest neighbors to look at
- **Odd K value is recommended** to avoid tie
- Each neighbor's vote can be weighted by the distance between the neighbor and the target
- KNN uses Euclidean, Manhattan 
## Naive Bayes
