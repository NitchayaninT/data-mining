# Introduction
## Data Mining Tasks💎
- ### Predictive or Supervised Learning
	- used for prediction
	- Construct a model of **independent variables** to **predict** dependent or **target variable (label)**
	- Input data is *labeled* 
	- **Classification**  : predict class labels, can be 0/1 or multi-class classification
	- **Regression** : predict numeric values
	- **Algorithms** : decision trees, logistic regressions, random forest, naive bayes
	
<img src="DataMining/pictures/Supervised-learning.png" alt="Alt text" width="500" height="300"/>

- ### Descriptive or Unsupervised Learning
	- used for analysis
	- input data is *unlabeled*
	- **Clustering** : group similar objects
	- **Association** : find strongly associated items
	- **Algorithms** : k-means clustering

<img src="DataMining/pictures/Unsupervised-learning.png" alt="Alt text" width="600" height="250"/>

## Choosing Data Mining Approach/Method
- **Variables** : attributes/features in our dataset (eg. male/female, age)
- **Data**
	- Potential Errors : Are there missing values, outliers?
	- Volume
	- Balanced/Imbalanced
- **Nature of candidate methods**
	- Goal of Analysis (Prediction or for Exploration/Analysis?)
	- How each algorithm works
	- Constraints
	- Parameters
- **Evaluation criteria**
	- Performance Metrics
	- Computational Cost
## Rapid Miner Basics

### Ex 1.1 Retrieve
fetches a dataset from our repository into our process so other operators can work on it

<img src="DataMining/pictures/Retrieve.png" alt="Alt text" width="400" height="200"/>

### 1.2 Read/Write files

<img src="DataMining/pictures/Read-and-write-files.png" alt="Alt text" width="550" height="300"/>

**Process 1**
1. Retrieve : Fetches/retrieves data from Samples folder
2. Write Excel : Write the dataset to the excel file
**Process 2**
3. Read Excel : After saving data to excel file, we can read data from excel file (outside files) and **import to repository**
**Process 3**
4. Read ARFF : import data stored in ARFF files, they are associated with Weka

**Read Operator VS Store Operator**
- **Read** : Loads data into our process **temporarily** so we can work with it, it pulls data from external source and the data **exists only in memory while the process is running**
- **Store** : **Permanently** keep the dataset inside **rapidminer repo**. We have to **read first and then store**, so that saves are loaded into repo in rapidminer's internal format (proper format for faster retrieval)
## Data Preparation
- **Data exploration** : Understand the dataset, its structure, quality and potential problems
- **Data wrangling** : Data cleaning, fix data quality issues to make the dataset reliable
- **Data transformation** : Prepare data in the **right format** and **scale** for ML algorithms (eg. normalization, extract date/time, aggregations)
- **Feature selection** : Identify the **most important attributes** to improve model performance and reduce complexity
- **Data sampling** : Manage **dataset size** and **balance** to make modeling efficient and fair
