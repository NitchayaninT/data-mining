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
==Cophenetic distance== = representative distance between a point in one cluster to a point in another cluster
**Ways to calculate** :
- ==single link== = shortest distance between points from different clusters
- ==complete link== = longest distance between points from different clusters
- ==group average== = average of all possible distances between clusters
- different linkage types give different clustering results

![[Pasted image 20251209001917.png]]
![[Pasted image 20251209001924.png]]


### Performance

