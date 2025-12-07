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
### PCA
PCA (Principal Component Analysis) is a **dimensionality reduction technique** and helps us to **reduce the number of features in a dataset** while **keeping the most important information.** It changes complex datasets by transforming correlated features into a **smaller set of uncorrelated components.**
![[Pasted image 20251207161214.png]]

- The data forms an **ellipse** (because the original features are correlated).
- **PC1** = the direction where data varies the MOST
- **PC2** = the next strongest direction, perpendicular to PC1
- PC1 ⟂ PC2 → they must be **uncorrelated**.

