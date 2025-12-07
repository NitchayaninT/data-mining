# Clustering Methods 1 (Prototype-based clustering)
- **Preprocessing** : PCA, normalization
- **K-means and K-medoids**
	- Clustering Steps
	- Centroids
	- Sum of squared error (SSE), Davis-Bouldin index (DBI)
- **EM (Expectation Maximization)**
	- Mixture models & assumption
	- Clustering steps, likelihood, log-likelihood
- **Gaussian Mixture**
## Preprocessing
### Normalization
- distance-based
- PCA works by analyzing variance, Features with large numeric scales dominate PCA, so need to do normalization before PCA
- And its also needed before distance-calculations like K-means
### PCA
PCA does **two things at the same time**
1. **Finds directions with the highest variance** (captures most info, cuz variance = how much data spread out/changes)
2. **Ensures each direction is uncorrelated (orthogonal) to all previous ones**
PCA (Principal Component Analysis) is a **dimensionality reduction technique** and helps us to **reduce the number of features in a dataset** while **keeping the most important information.** It changes complex datasets by transforming correlated features into a **smaller set of uncorrelated components.**
![[Pasted image 20251207161949.png]]

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
## K-Means
- Find K-clusters that are represented by centroids
**Process**
- Select K data points as initial centroids
- For each cycle
	- For each data point X
		1. Calculate the distance from X to each centroid
		2. Assign X to the closet centroid
	- Complete 1 cycle, all points are put in clusters -> **update centroids**
	- **Calculate SSE, DBI**
- until stopping criteria (centroids are stable, SSE or DBI is stable)
- cycles completed
### Measurements
- Use Euclidean distance to calculate distances
- **Centroid = mean of all points** in the cluster
	- centroid = (mean of A1, mean of A2, ... mean of Am)
- **SSE = Sum of squared error**
![[Pasted image 20251207165335.png]]
measure the difference between the observed data points and the values predicted by a statistical model.
	- Objective = **minimize SSE**
	- Average within-cluster distance = sqrt(SSE)/K
- **Davies-Boundin Index (DBI)**
	- 