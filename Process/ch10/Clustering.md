# Clustering Methods 1
- **Preprocessing** : PCA, normalization
- **K-means and K-medoids**
	- Clustering Steps
	- Centroids
	- Sum of squared error (SSE), Davis-Bouldin index (DBI)
- **EM (Expectation Maximization)**
	- Mixture models & assumption
	- Clustering steps, likelihood, log-likelihood
- **Gaussian Mixture**
## ==Preprocessing==
### Normalization
- distance-based
- PCA works by analyzing variance, Features with large numeric scales dominate PCA, so need to do normalization before PCA
- And its also needed before distance-calculations like K-means
### PCA
PCA does **two things at the same time**
1. **Finds directions with the highest variance** (captures most info, cuz variance = how much data spread out/changes), PC1 = the direction where the **data spreads out the most**
2. **Ensures each direction is uncorrelated (orthogonal) to all previous ones**, PC2 has to be orthogonal to PC1, PC3 orthogonal to PC1,2, vice versa
PCA (Principal Component Analysis) is a **dimensionality reduction technique** and helps us to **reduce the number of features in a dataset** while **keeping the most important information.** It changes complex datasets by transforming correlated features into a **smaller set of uncorrelated components.**
![[Pasted image 20251207161949.png]]
** PCx = a combination of all attibutes
**How does it help clustering?**
- PCA reduces the dataset to fewer, uncorrelated components, **captures most of the variance and removes noise**
- Clustering algorithm like K-means run on PCA-transformed data
	- This means clustering happens on PC1, PC2, PC3, … instead of the original features.
	- PCs are uncorrelated → distance calculations are more meaningful
	- Dimensionality is lower → **clusters become more separated**
	- Noise is removed → fewer false clusters
K-means uses **Euclidean distance**.  
- Euclidean distance works best with **independent features**
- when features are correlated (move together), distance becomes distorted, because it calculates distances from correlated attributes while they literally move together, so it creates a **fake distance difference**
Using PCA fixes this.
### K-Means with PCA
- Once we apply PCA, we get a new table with new features 
	- PC1, PC2, ..., PCk
**These PC features do NOT correspond to original attributes but they are _linear combinations_, compressed representations.**
- once we use PCA for clustering or classification, the cluster labels or predictions exist ONLY in the PCA-transformed space.
Example:
1. You transform original data → PCA table
2. You run clustering on PCA table → **cluster labels assigned here**
3. Now you want to know:
	- What are the **original attribute values** inside each cluster?
	- What original attribute contributed to each cluster?
	- Which original variable is large/small for cluster 1, cluster 2…?

But PCA table **does not contain the original attributes anymore**, so we have to join with original table, which gives like ==x1, x2, x3, ..., xd, PC1, PC2, ..., PCk, cluster_label==
![[Pasted image 20251207212807.png]]
**Clusters**
![[Pasted image 20251207212822.png]]

# Prototype-based clustering
- each cluster is represented by a central object, called prototype or centroid
- Compare **each object with all cluster centroids** and assign the object to the cloest one -> using similarity or **distance function**
- Cluster shape = **globular**, with centroid at the middle
- Clustering methods = **K-means, K-medoids**
## ==K-Means==  
- Find K-clusters that are represented by centroids
**Process**
- Select K data points (in RANDOM) as initial centroids
- For each cycle
	- For each data point X
		1. Calculate the **Euclidean distance** from X to **each centroid**
		2. Assign X to the **closest centroid**
	- Complete 1 cycle, all points are put in clusters -> **update centroids** by calculating the **mean of all points within the cluster** (centroid is an **imaginary** point)
	- **Calculate SSE, DBI**
- until stopping criteria (centroids are stable, SSE or DBI is stable)
- cycles completed
### Measurements
- Use Euclidean distance to calculate distances
- **Centroid = mean of all points** in the cluster
	- centroid = (mean of A1, mean of A2, ... mean of Am)
	
- **SSE = Sum of squared error**
![[Pasted image 20251207165335.png]]
 It is calculated by summing the squared distances **between each data point and the centroid** (mean point) of the cluster to which it has been assigned.
	- Objective = **minimize SSE**, data points are more compacted within the cluster
	- Average within-cluster distance = **sqrt(SSE)/K**
	
- **Davies-Bouldin Index (DBI)**
 ==a metric for evaluating the quality of a clustering algorithm's results==. It is an internal validation scheme that measures **how well the clusters are separated** and **how compact they are**. A **lower DBI value indicates a better clustering solution**.
 - Rij = Ratio between **cohesion & separation** of clusters i and j
 ![[Pasted image 20251207171359.png]]

- **We want the ratio to be small** -> objects in the same clusters are close to each other (cohesion) and clusters are well separated (separation)
- Worst case scenario for cluster i is to consider the **maximum Rij**
	- Di = max{**Ri1, Ri2, Ri3,...**}; considering all other clusters j
	- Di = What is i cluster's **worst pair**? (max Rij)
- For K-clusters :
	- **DBI = (sum of Di for all clusters)/ K**
- **Objective** = minimize DBI
### Performance (SSE vs DBI)
- **SSE** : measure**s if data points are more compacted with the cluster** (low SSE), closer to centroid
- **DBI** : A ratio of **cluster compactness** and **cluster separation** (ideal : nominator low, denominator high). Its used to balance tight clusters and well-seperated clusters
## ==K-Medoids==
Calculate average distance **from each point to all other points within the cluster**
**New centroid** = point with min avg distance 
Centroid is the **best actual point** in the cluster
### K-Means VS K-Medoids
Example : these objects are in the same cluster
d12 = distance from point 1-2
![[Pasted image 20251207174146.png]]

**Centroid in K-Means**
- avg of x = 8.2, avg of y = 10.7
- centroid = (8.2, 10.7), imaginary
- sensitive of outliers because **outliers pull centroid away** from where they should be
**Centroid (Medoids) in K-Medoids** 
- avg of distance from each point (obj) to all other points in within the cluster
- Choose **object 2** as centroid since it has the **min avg distance out of all points**, meaning its likely to be at the center of the cluster
- More robust to outliers because outliers are far from other points & **will never be picked as centroids** , cuz we pick the lowest mean distance
## Choosing K (No. of clusters)
- **SSE + elbow** : plot SSE against K and pick K at the elbow
	- no. of clusters (k) increases, **points getting closer to centroid** (low SSE)
	- we pick at the elbow because thats the point right before the SSE decreases very slowly
- **DBI + min point** : plot DBI against K and pick K at minimum DBI
	- at minimum = best trade-off between cohesion & seperation
	- higher k doesnt necessary mean lower DBI, even tho it makes points getting closer to centroid like SSE but it can **make the clusters closer to each other** (bad seperation)
![[Pasted image 20251207205537.png]]

**K-Means can be used with nominal attributes** 
- By converting **nominal to numerical** (nominal attribute is seperated into subattributes)
- but the original nominal attributes dont exist on the transformed table, so after clustering is done on the transformed table, JOIN with the original table so that we can know the **statistics descriptions for each cluster**, like average/mode of sex, passenger class **within the cluster**
## Limitations of K-Means
Distortion due to outliers
- Detect & Eliminate them prior to clustering
- Use K-Medoids instead
Cannot find **natural clusters that have different sizes**, densities or **non-globular shapes**
1. K-means minimizes **sum of squared distances** (SSE), so **big clusters dominate the objective** because the distances between centroid and the data points in big clusters are **HIGHER**, so some of the data from bigger cluster gets seperated into smaller clusters in order to **reduce SSE**
![[Pasted image 20251207214653.png]]
2. Sparse cluster’s centroid ends up in the wrong place
- Dense cluster **attracts multiple centroids**
- Points from the sparse cluster may get pulled into dense cluster boundaries
K-means has no mechanism to handle density differences because it uses simple Euclidean distance.
![[Pasted image 20251207214937.png]]
3. K-means implicitly assumes clusters look like **spheres/blobs** (globular).
This is because **distance to centroid defines the boundary**

![[Pasted image 20251207214946.png]]

**Solution** -> Increase K to obtain smaller & purer clusters. Then merge similar or related clusters
![[Pasted image 20251207220625.png]]
# Probability-based clustering
A cluster of objects is assumed to be drawn from a certain population. So it is represented by **distribution model of that population**, eg. normal distribution with **μ** and **𝜎2** parameters
- Each object is assigned to the **most likely cluster**
- Clusters can have any shape, any density
- **Clustering methods** : EM, Gaussian Mixture
## ==Expectation-Maximization (EM) & GMM==
**Cluster model (GMM)** = normal distribution (mean & variance)
- Each cluster is assumed to be the representative of each population
- Each population has **normal probability distribution** or data model with **μ** and **𝜎2** parameters
- Consider 2 clusters A,B = **models ->  N(μA ,𝜎2A) and N(μB ,𝜎2B)** 
	- **mean** = cluster center
	- **var ^2** = how spread out the cluster is (shape)
	- var and mean are in (x,y) corrdinates
This is why EM/GMM clusters sometimes look like ellipses, not circles.
- Given a value x, the **probability that it comes from each cluster** : 
![[Pasted image 20251208002325.png]]
- **P(x) is removed** so that the numerators don't sum to 1
- **P(x | A) P(A)** is called the ==likelihood that x is in cluster A==
### Likelihood
P(x|A) is calculated from a **normal distribution function**
![[Pasted image 20251208004206.png]]
- Total likelihood of x = **P(x|A)P(A) + P(x|B)P(B)** 
**If x is in the correct cluster, likelihood should be high**, because if its in cluster A, the contribution to P(A) is high, so it dominates whats added in B term (x is near A's mean)
- if x is not near A's nor B's mean, P(x∣A) and P(x∣B) are **not high** = low likelihood
- When **normalized**, the likelihoods of x sum to 1
### Performance = log-likelihood
maximize
**Total likelihood** (likelihood of independent events) = the **likelihoods of all points multiplied together**. Log function is applied to make the calculation faster because the multiplication of all likelihood tends to be a bunch of decimal points
- How well does the **mixture model explain ALL the data?**
	- If every point has high density under the model → big likelihood.  
	- If some points fall in low-density regions → small likelihood.
### EM steps
Random initial  **μ and 𝜎** for K clustering models
For each cycle
- For each data point X
	1. **Calculate the likelihood that it comes from each cluster**
	2. Assign X to cluster with the highest likelihood
- Complete 1 cycle (i.e. all points are put in clusters) -> update **μ and 𝜎** 
- Calculate log-likelihood for all points
Until stopping criteria (eg. parameters or log-likelihood is stable)

![[Pasted image 20251208122421.png]]
Score = inverted likelihood = tend to be outlier of cluster 0, far from gaussian center
### Choosing K
**Choosing K** = log-likelihood + reverse of elbow
- Log-likelihood ALWAYS **increases as K increases**
- Always goes up, becomes less steep (improvement small)→ looks like a **reverse elbow**.
This is guaranteed because:
- More clusters = more parameters
- More parameters = more flexibility
- More flexibility = better fit
- **Better fit = higher log-likelihood**
Choose K that : is near the reverse elbow AND has the highest BIC, BIC adds a penalty for having too many clusters (balances better fit VS complexity), so BIC chooses the **simplest model that still explains the data well.**