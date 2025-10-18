# Data Preprocessing✨
Topics
- **Preprocessing Attributes**
	- Value Manipulation
	- Normalization
	- Type Conversion
- **Discretization**
	- Unsupervised
	- Supervised
		- Entropy
		- Information Gain
- **Handling Potential Problems**
	- Noises and Outliers
	- Missing Values
		- Elimination
		- Imputation (estimate missing values)
- **Preprocessing records & tables**
	- Sampling & record manipulation
	- Table Manipulation
- **Feature Selection & Aggregation**
	- Attribute Scoring & Subset selection
	- Principle Component Analysis (PCA)

## Preprocessing Attributes
### Value Manipulation
---
**Nominal Values**
- Cut
- Merge
- Replace
- Trim
**Both Nominal & Numeric Values**
- Map
- Set

### Normalization
---
Different range of attribute values affect distance-based methods such as KNN, K-means clustering
Normalization **transform numeric attribute** by **changing its scale** or range to a new scale
- ==Min-Max normalization==
- Decimal scale normalization
	- **old value / d** ; where d = 10,100,1000,...
- ==Standardization (Z-score normalization)== 
	- new values have **mean = 0, stdev = 1**
- ==Interquartile range normalization==
	- extreme values beyond Q1 and Q3 are excluded
	- **newValue = (oldValue-median)/(IQR/1.349)** ; IQR = 1.349 times of SD
- ==Proportion normalization==
	- newValue = oldValue/Sum of all attribute values

### Type Conversion
---
- ==Nominal -> Binary (binarization)==
	- **Binary encoding** = map each category to unique binary code
		- **One-hot encoding** = map each category to each binary code (RapidMiner uses this approach, expanded to 5 columns with each category)
	
![[one-hot-encoding.png]]

Category 1 (x1) : Programmer
Category 2 (x2) : System Analyst
Category 3 (x3) : Network admin
Category 4 (x4) : Technician
Category 5 (x5) : Project Manager
- ==Nominal or binary -> Numeric==
	- Approach 1 : Binary attribute first (one hot encoding), then map binary values to {0,1}
		**Example**
		- Original Attributes 
			- Passenger class = {first,second,third}
			- Sex = {female,male}
		- Binarization
			- Passenger class = first -> encoded as {0,1} (1 if first, 0 if not)
			- second, third, male, female also **encoded as {0,1}**
		- More appropriate because **original proportions are preserved**
		- RapidMiner methods : **Nominal to Binominal & Nominal to Numerical**

	- Approach 2 : Map categories to numeric values
	
	![[categoriesToNumeric.png]]
## Discretization
Converting a continuous attribute (like age, income, temperature) into categorical bins
Example : 
- age : 18,22,35,70
- After discretization : young, adult, senior
### Unsupervised
---
Partition data into **bins**, don't use class labels
- ==Equal Width==
	- Create bins of equal range
	**Example**
	- Given 2 bins, set boundaries min = 1, max = 5
	- Bin 1 : range [1,2.5]. Bin 2 : range [2.5,5]
- ==Equal Frequency==
	- **By frequency** : Create bins of equal frequency
	- **By size** : Given bin size, create bins of equal size
	- Ranges of different bins may vary
	**Example** By freq
	- Given 2 bins, didnt set boundaries
	- Bin 1 & Bin 2 size : size/2
	**Example** By Size
	- Given Size of bins = 50, set sorting direction (decreasing/increasing)
	- total size / size of bins = number of bins
- RapidMiner operations : **Discretize by binning, Discretize by freq, Discretize by size, Discretize by User (user set range by themselves)**
### Supervised
---
Entropy-based method. Use **class label** to supervise discretization
- ==Entropy== = measurement of **information purity**
	- High information (purity) -> low entropy (energy)
	- In pure bin = all records are from only 1 class
	- Entropy of each bin = **class proportion** of each bin
	
![[entropyFormula.png]]

	- p1,p2... = class proportions in the bin

- ==Information Gain== = entropy before split - entropy after split
	- If partition is good = lower entropy & higher information gain

**Example**

![[entropyCalculation.png]]

- It picks the cut that **MAXIMIZES information gain**
- RapidMiner Operations : **Set role (label), Discretize by entropy**
- When to use entropy-based?
	- **To get meaningful splits** -> each class has proper amount of data for a classifier to learn the pattern of that class
## Feature Selection & Aggregation
Identify important attributes for modeling and remove useless ones for better visualization & results
**Attributes that can be filtered out** :
- Correlated value
- irrelevant to data analysis task
- very low variance or SD (almost the same value for every record)
- very high variance or SD (all values are unique)
**Approaches** : 
- Attribute scoring/ranking
- Subset Selection
- Principle component analysis (PCA)
### Attribute scoring/ranking
 ---
 Calculate the scores of all attributes by using some criteria & select attributes with highest scores
 Measure **"relevancy"** of each attribute with respect to class label
 - Based on **correlation** between each attribute and class
	 - ==Chi-square== for **nominal attr**. its used to calculate weight of each attr and keep only attr with high weights
	 - ==Correlation (Pearson)== for **numeric attr**. 
 - Based on **impurity measurement** when using each attribute to split data into correct classes. pure bin = all records are from only 1 class = zero entropy
	 - ==Gini== (Generalized Inequality Index) : 0 means perfect equality (pure bin), 1 means perfect inequality (impure bin), like entropy
	 - ==Information gain== : entropy before split - entropy after split
	 - ==Gain ratio== : information gain/split penalty
	 - ==Symmetrical uncertainty== 
 - Other partition based methods
	 - ==Relief== : compare probabilities that **nearest records** in the same class and in different classes **have different attribute values**. If the attribute's value is very different between the instance and a miss (different class), it's a good attribute, so its score is increased. Otherwise, score decreases
 - By-products of **classification methods**
	 - OneR : 
	 - Random Forest :
	 - SVM : 
	 
### Subset Selection
---
Choose an attribute to add to or remove from a subset, to improve classification performance -> higher accuracy or the same accuracy but with fewer attributes
- **Forward Selection** 
	- From empty set, keep adding attributes one by one
- **Backward elimination**
	- From complete set, keep removing attributes one by one, may suffer from overfitting