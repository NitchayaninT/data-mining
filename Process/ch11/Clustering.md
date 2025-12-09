# Clustering Methods 2
- **DBSCAN** 
	- **density-based**
	- Point types : core, border, noise
	- Epsilon, MinPts
	- Choosing epsilon
- **HAC** 
	- distance-based + hierarchical structure
	- cophenetic distance : single link, complete link, group avg
	- performance

## Density-based clustering
- A **cluster** is defined as a **dense region** where objects are concentrated, surronded by a low-density area where objects are sparse
- Objects in low-density area are **discarded as noises**
## DBSCAN
- Density-based spatial clustering of applications with noise
- Determine whether each data point is in **dense region (cluster)** or **sparse region (noise)** by using **2 parameters**
	- **Radius/epsilon**
	- Minimum points (MinPts) **threshold**
Example : **set MinPts threshold = 7**
Ideal : start with 4
![[Pasted image 20251208230750.png]]
**for each point,** draw a circle of given radius and count #points inside the radius
- Radius(x) = 8 -> core point
- Radius(y) = 4 -> border point (y is still **inside the radius of core point, x**)
- Radius(z) = 3 -> noise
### Type of data points
- **Core point** -> pass MinPts threshold. Assumed to be at the core of **dense region/cluster**
- **Boarder point** -> fail MinPts threshold. but it is inside the **radius of other core point.** It is on the **edge of cluster**
- **Noise** -> not core point & not boarder point. It is in sparse region
### Clustering Steps
1. Label all points as **core points, boarder points, noises**
2. Remove noises 
3. Form clusters **by adding links between core points that are within the radius of each other**
4. **Assign each boarder point to cluster** whose radius covers it. If a border point is covered by > 1 clusters, the decision can be based on
	- Random selection
	- Actual distance : pick nearest cluster
	- Cluster density : pick cluster with highest density
### Choosing epsilon
**Choose epsilon** = distance to K-th neighbor **(K = minPts-1)**
Find distances at the *k-th nearest neighbors of all points*. sort distances and plot graph (k-distance graph) -> **pick distance at (or slightly after) elbow point**
- **X axis** = **points** sorted by distance (point no.1,2....,3000)
- **Y axis** = **distance** to 4th nearest neighbors
Small 4th neighbor distances = **dense**. Large 4th neighbor distance = **noise**.
![[Pasted image 20251208232310.png]]

|          | **MinPts**                                                           | **Epsilon**                                                                                                                                    |
| -------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Too high | **Harder to become a core point**, valid becomes noise               | **Noise** has big radius to cover enough points to pass MinPts. **So it is labelled as core point.**                                           |
| Too low  | **Noise passes the threshold easily** and is labelled as core point. | Radius of a valid point **doesn't cover enough points (to pass MinPts)** to be core and it isn't border point. So **it is labelled as noise.** |

## HAC
Hierachical structure + distance-based
- **Output** = nested clusters, represented as dendrogram
- no need to specify K (any is required). **==No of clusters can be obtained by cutting a dendrogram==**

![[Pasted image 20251209001204.png]]

### Clustering approach
- start with points as individual clusters
- repeatedly **merge closet clusters** (according to ==cophenetic distance==) until only one outermost cluster is obtained
### Linkage types
==Cophenetic distance== = **representative distance** between a point in one cluster to a point in another cluster
**Ways to calculate** :
- ==single link== = shortest distance between points from different clusters
- ==complete link== = longest distance between points from different clusters
- ==group average== = average of all possible distances between clusters
- different linkage types give different clustering results

![[Pasted image 20251209001917.png]]
![[Pasted image 20251209001924.png]]

#### ==Single Link==
- list all x,y coordinates of all data points
- Start with points as **INDIVIDUAL CLUSTERS**
- Euclidean distance formula = `d(A, B) = sqrt((x2 - x1)² + (y2 - y1)²)`
- Draw distance matrix
![[Pasted image 20251209202352.png]]

- The **shortest distance** between any two clusters (or points) is 0.11, which is from **p3 to p6**
- **Merge p3+p6** (merge **closest clusters**) until everythings merged into big cluster (not yet)
	- **How to merge + update Distance matrix** : (Calculate ==cophenetic distance== between p3+p6 and other clusters)
		- d(1, 3+6) = min{ d(1,3), d(1,6) }
			- min {0.22, 0.23} = **0.22** -> cophenetic distance 
		- d(2, 3+6) = min{ d(2,3), d(2,6)}
			- min {0.15, 0.25} = **0.15**
		- d(4, 3+6) = min{ d(4,3), d(4,6)}
			- min {0.15, 0.22} = **0.15**
		- d(5, 3+6) = min{ d(5,3), d(5,6)}
			- min {0.28, 0.39} = **0.28**
![[Pasted image 20251209202803.png]]

- now we have p3+p6 as a ==new cluster==
- From new distance matrix -> merge points with shortest distance again, which is p2 and p5
- Update distance matrix + Form new cluster
	- d(1, 2+5) = min{ d(1,2), d(1,5)}
		- min{0.24, 0.34} = **0.24**
	- d(3+6, 2+5) = min{d(3+6,2), d(3+6,5)}
		- min{0.15, 0.28} = **0.15**
	- d(4, 2+5) = min{ d(4,2), d(4,5)}
		- min{0.20, 0.29} = **0.20**

![[Pasted image 20251209204428.png]]

- p2+p5 is a new cluster, now (p2+p5) and (p3+p6) have the **shortest distance** so they should join as a new cluster 
	- d(1, 2+5+3+6) = min(0.24, 0.22) = **0.22**
	- d(4, 2+5+3+6) = min(0.20, 0.15) = **0.15**
- so,  the matrix table will be p1 | p2+p3+p5+p6 | p4
- do this until every cluster is merged

#### ==Complete Link==
![[Pasted image 20251209210441.png]]

- Form new cluster by choosing the shortest distance between 2 clusters (0.14), just like single link -> **merge p5 & p2 together**
- But the method of choosing ==cophenetic distance== is different
	- Calculate ==cophenetic distance== between **p5+p2 and other clusters** BY choosing the maximum distance between every cluster and cluster p5+p2
		- d(1, 5+2) = max(0.34, 0.24) = 0.34
		- d(3+6, 5+2) = max(0.25, 0.39) = 0.39
		- d(4, 5+2) = max(0.29, 0.20) = 0.29
	- Then, update distance matrix again

#### ==Group Average==
![[Pasted image 20251209212257.png]]
- How it forms a cluster is the same as the other 2, But the method of choosing ==cophenetic distance== is different
	- Here, it calculates the average distance between the **merging cluster and other points.** 

### Cluster shape
- ==Single link== produces **clusters of any shape**. Cluster sizes can be unequal
	- it only cares abt tiny bridge between clusters
	- Even though the shape is irregular or long, as long as **any two points are close**, it merges. and a large cluster can absorb a small cluster **via a tiny distance.**
- ==Complete link== tends to produce **globular clusters** of equal sizes (big natural clusters are broken into smaller ones)

![[Pasted image 20251209215929.png]]

In this example, when complete link merges a cluster with the closest cluster, it calculates cophenetic distances from the merging cluster with others by choosing **max distance**. like (d1,d2), (d3,d4). it links d1 and d4 with each other cuz its the furthest distance. BUT...when merging a new distance, from (d1, d6) is shorter than (d1,d4), so merge (d1,d2) with (d5,d6) instead!
### Effect of noises & outliers
- **Single link** clusters **can be distorted by noises**
	- Noises effect points around them and cause **chaining effect**
	- noise doesnt belong to any cluster
![[Pasted image 20251209221147.png]]
	- Green and yellow points are bridged by noises (orange), they are put in 1 cluster and clusters of yellow points are broken
- **Complete link** clusters **can be distorted by outliers**
	- Distance to an outlier becomes ==cophenetic distance== (max), so outliers may be merged early (into a valid cluster)
	- outlier is usually far from others
	- d2 is merged with outlier (d1) instead of cluster d3,4,5
![[Pasted image 20251209221308.png]]
- **Group average**
	- Less suspectible to noises and outliers
	- but also tend to produce globular clusters
	![[Pasted image 20251209222804.png]]
![[Pasted image 20251209222628.png]]

- single link can produce non globular clusters, other links favor globular clusters

### Where to cut the dendrogram to get clusters?
![[Pasted image 20251209223320.png]]
Cutting at the red dashed line labeled **K = 3** gives **3 clusters**.
- Because of _chaining_, the clusters are **uneven, elongated, and sometimes unintuitive**.
- The rightmost and leftmost branches ==merge ONLY at a very high height==
![[Pasted image 20251209223327.png]]
- Cutting at the top dashed line gives **K = 2** (two compact clusters).
- Cutting at the lower dashed line gives **K = 4** (four balanced, globular clusters).
- Because of **maximum distance rule**, branches join only when _all_ points are close, so shapes are **rounder and more compact**.
### Performance : Cophenetic Correlation
- Correlation between **real distances and cophenetic distances**
- ==Cophenetic distance== = dendrogram height at which points i and j **first merge** into the same cluster
- Two dendrograms (e.g., single-link vs complete-link) may produce **very different shapes**, and we need a way to say:
	- “Which one **fits the real data better**?”
	- “Which **linkage method is more accurate** for this dataset?”
- **High correlation** = cophenetic distances are close to real distances, points that are close in real data join early, and points that are far join late = **good result**
- ==Complete link usually gives higher Cophenetic correlation than Single link== because single link only uses **minimum** distance between clusters.
	- This causes **chaining**:
	    - Far-away points may be merged too early (merge through a chain of small distances), so in the dendrogram, they are connected early **(become cluster early)** even tho they are "actually" far apart

