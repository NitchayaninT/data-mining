## Operations
- ### Data access
	- Retrieve
	- Store
	- Rename Repository Entry
	- Copy Repository Entry
	- Move Repository Entry
	- Delete Repository Entry
	- Store to File 
	- Retrieve From file
	- Files (folder)
	- Database (folder)
	- Applications (folder)
	- Cloud Storage (folder)
- ### Blending
	- Attributes
	- Examples
		- Filter
	- Table
		- Aggregate
		- Pivot
	- Values (folder)
		- Merge
		- Cut
		- Replace
		- Trim
		- Map
		- Add
		- Split
- ### Cleansing
	- Normalization (folder)
	- Binning (folder)
	- Missing (folder)
	- Duplicates (folder)
	- Outliers (folder)
	- Dimensionality Reduction (folder)
	- Statistics
	- Quality Measures
- ### Modeling
	- Predictive (folder)
		- Lazy
		- Bayesian
		- Trees
		- Rules
		- Neural Nets
		- Functions 
		- Logistic Regression
		- Support Vector Machines
		- Discriminant Analysis
		- Ensembles
	- Segmentation (folder)
		- k-means
		- Support vector clustering
		- Random clustering
		- Agglomerative clustering
		- Top down clustering
		- Flatten clustering
		- Extract cluster prototypes
		- DBSCAN
		- Cluster model visualizer
	- Associations (folder)
	- Correlations (folder)
	- Similiarities (folder)
	- Feature Weights (folder)
		- Weight by Information gain
		- Weight by information gain ratio
		- Weight by rule
		- Weight by Value Average
		- Weight by Deviation
		- Weight by Correlation
		- Weight by Chi squared statistic
		- Weight by Gini Index
		- Weight by Tree Importance
		- Weight by Uncertainty
		- Weight by Relief
		- Weight by SVM
		- Weight by PCA
		- Weight by Component Model
		- Weight by User Sepecification
		- Data to Weights
		- Weights to Data
	- Optimization (folder)
		- Parameters (folder)
		- Feature Selection (folder)
			- Forward Selection
			- Backward Elimination
			- Optimize Selection
		- Feature Generation
		- Feature Weighting
	- Time Series (folder)
- ### Scoring
- ### Validation
	- Performance (folder)
		- Predictive (folder)
			- Performance (Classification)
			- Performance (Binomial Classification)
			- Performance (Regression)
			- Performance (Costs)
			- Performance (Ranking)
		- Segmentation (folder)
			- Cluster Count Performance
			- Cluster Distance Performance
			- Cluster Density Performance
			- Item Distribution Performance
			- Map Clustering on Labels
		- Significance Tests (folder)
		- Performance
		- Extract Performance
		- Performance (user-based)
		- Performance (Min-Max)
		- Performance to data
		- Multi Label Performance
	- Visual (folder)
	- Cross Validation
	- Split Validation
	- Bootstrapping Validation
	- Wrapper Split Validation
	- Wrapper-X-Validation
- ### Utility

## Ports
### Data & Example Ports
|**Port**|**Full Name**|**Type**|**Meaning / Usage**|
|---|---|---|---|
|`exa`|ExampleSet|Data|A dataset (rows = examples, columns = attributes). This is the most common data port.|
|`exs`|ExampleSet (Synonym)|Data|Same as `exa` — used in some older operators.|
|`par`|Partition|Data|Subsets of data (like training/test splits from “Split Data”).|
|`unl`|Unlabeled Data|Data|A dataset **without** a label attribute (used for prediction).|
|`lab`|Labeled Data|Data|A dataset **with labels** (actual or predicted).|
|`app`|Append / Application Data|Data|Used when merging or applying transformations.|
|`sel`|Selection|Data|Used when selecting specific examples or attributes.|
|`exa out`|ExampleSet Output|Data|The resulting modified or filtered dataset.|
|`exa in`|ExampleSet Input|Data|The dataset coming into an operator.|
### Model & Learning Ports
|**Port**|**Full Name**|**Type**|**Meaning / Usage**|
|---|---|---|---|
|`tra`|Training Data|Data|Input data used for training models.|
|`mod`|Model|Model|A trained model (Decision Tree, SVM, Naive Bayes, etc.).|
|`mod out`|Model Output|Model|Outputs the model to be used later.|
|`mod in`|Model Input|Model|Inputs a model for further processing or applying.|
|`wei`|Weights|Data|Example weights — used in weighted models or cost-sensitive learning.|
|`pre`|Preprocessing Model|Model|Represents transformations applied before modeling (like normalization).|
|`fea`|Feature Set|Model|Contains selected or generated features.|
|`ens`|Ensemble Model|Model|Combination of multiple models (e.g., bagging, boosting).|
### Performance/Evaluation ports
| **Port**    | **Full Name**          | **Type**   | **Meaning / Usage**                                               |
| ----------- | ---------------------- | ---------- | ----------------------------------------------------------------- |
| `per`       | Performance            | Evaluation | Contains performance metrics (accuracy, precision, recall, etc.). |
| `per (avg)` | Averaged Performance   | Evaluation | Combined or averaged metrics across folds (in cross-validation).  |
| `per (val)` | Validation Performance | Evaluation | Performance on validation data.                                   |
| `val`       | Validation Data        | Data       | Data used for validation in model evaluation.                     |
| `tes`       | Test Data              | Data       | Data used for final testing.                                      |
### Process control / meta ports
|**Port**|**Full Name**|**Type**|**Meaning / Usage**|
|---|---|---|---|
|`inp`|Input|Control|Input signal to trigger execution or pass data to subprocesses.|
|`out`|Output|Control|Output signal or data flow from a subprocess.|
|`res`|Result|Control|Used in macros and sub-processes to output final results.|
|`sub`|Subprocess|Control|Used in “Loop” or “Optimize” operators to run a sub-process.|
|`log`|Log|Control|Used to log messages or store process results.|
### Attribute / feature engineering ports
|**Port**|**Full Name**|**Type**|**Meaning / Usage**|
|---|---|---|---|
|`att`|Attribute|Metadata|Refers to attribute structure (column information).|
|`exa/att`|ExampleSet / Attribute|Mixed|Carries both data and metadata about attributes.|
|`gen`|Generated Attributes|Metadata|Output after attribute generation (like polynomial or normalized features).|
|`agg`|Aggregated Data|Data|Output of grouped or summarized attributes (e.g., after aggregation).|
### Text / Data Mining - specific ports
|**Port**|**Full Name**|**Type**|**Meaning / Usage**|
|---|---|---|---|
|`doc`|Document|Text|Text document or collection of documents.|
|`tok`|Token|Text|Output after tokenization.|
|`vec`|Vector|Text|Vectorized text representation (e.g., TF-IDF).|
|`cor`|Corpus|Text|Entire text corpus used for NLP tasks.|
