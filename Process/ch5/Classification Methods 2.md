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
Record 1 = 1 0 0 0 0 0 0 0 0 0
Record 2 = 0 0 0 0 0 0 1 0 0 1
- **Distance : Hamming**

![[Screenshot 2025-10-16 at 12.51.18.png]]

- **Similarity : Simple Matching Coefficient (SMC)**
![[Screenshot 2025-10-16 at 12.51.35.png]]



### Distance between numeric values
### Similarity between numeric values
## K-Nearest Neighbors (KNN)

## Naive Bayes
