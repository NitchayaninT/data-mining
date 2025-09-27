# Data Preprocessing✨
Topics
- **Preprocessing Attributes**
	- Value Manipulation
	- Normalization
	- Type Conversion
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