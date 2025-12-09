# Outliers
- **Distance-based**
	- **K** (distance to K-neigbor), effect of K being too high/too low
- **Local outlier factor**
	- **LOF score**
- **By-product of clustering**
	- **EM/GM** -> log-likelihood vs Invert log-likelihood
	- **DBSCAN** -> noises
	- **HAC** -> dendrogram
- given clustering output or outlier scores -> ==identify outliers== (from records of LOF score, EM/GM,...)
## Distance-based Detection
Find data points that are significantly far from others
Using ==KNN== concept
- Calculate distance from **every point** to its **k-th nearest neighbor**
- Sort the distances. Points with **large distances** are likely to be outliers
- Effect of k
	- **Too small** -> a few k outliers that are close to each other are **not detected as outliers**
	- **Too big** -> a **small group of valid data** (#members < k) that are well separated from other clusters are **mistaken as outliers**
- example : if K=5, outliers = top N **points** that are farthest from their **5th nearest neighbors**
## Density-based Detection (simple)
Identify data points in low density as outliers
**Given 2 parameters**
- distance (d)
- Proportion of data points (p) eg. 99%
- X = outliers if at **least 99% of data points are outside its radius d**
	- Small d -> more outliers are detected because more points (> p%) are outside the radius
	- Big d -> fewer outliers are detected because of the opposite
### Local Outlier Factor (LOF)
“Is point X in a region with ==LOWER density than its neighbors?==”
Simple density-based method uses 2 global parameters **d and p**
- It ==cannot== handle cases when **data have different densities**
- d may be too high for some regions, but too low for the others

LOF compares local density of X with densities of its K neighbors
![[Pasted image 20251209233537.png]]
LOF = 1 -> X has similar density as neighbors
LOF < 1 -> X has higher density. It is likely to be cluster's core
LOF > 1 -> X has lower density. It is ==likely to be outlier==

**Local density (p)**
- If p is in a **dense** region → distances small → density large
- If p is in a **sparse** region → distances large → density small
After that, compare density of X with density of its K neighbors 
## Clustering-based Detection
Observe **result from clustering**
**Candidate outliers** : points that dont belong to any cluster or too far from closest centroid, small clusters that are very far from other valid clusters -> all points in small clusters may be outliers
**By-product of clustering**
	- **EM/GM** 
		- If a point has **very low likelihood** under _all clusters_, it is likely an outlier.
		- point x is **far from all means** or lies in a **low-density region**
		- log-likelihood vs Invert log-likelihood
			- Outliers detection methods **==usually want higher score as outlier==**, but log-likelihood is (low = outlier), so they invert it to "Invert log-likelihood"
	- **DBSCAN** 
		- check whether **noises have extreme values** (DBSCAN already labeled noises)
	- **HAC**
		- outliers are likely to be merged later (when using single link), high dendrogram height. Cut dendrogram to get a small cluster of outliers
![[Pasted image 20251210001325.png]]
Cut the points at K=5 because these data points have very high dendrogram heights. -> prob outliers

given clustering output or outlier scores -> ==identify outliers== (from records of LOF score, EM/GM,...)


