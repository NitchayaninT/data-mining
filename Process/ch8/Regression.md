# Regression
- **Numeric Prediction**
	- **Linear Regression**
	- **Extensions of Classification methods**
		- Classification and regression tree (CART)
		- SVR
		- Neural network
	- Evaluation metrics
- **Classification**
	- **Logistic Regression**
## Linear Regression
- Goal : find best coefficients b0, b1, ..., bm that **minimize the error between predicted and actual y**
- Consider multiple predictors or **attributes** A1, A2, ..., Am
- **Regression model** is `y = b0 + b1A1 + b2A2 + ... + bmAm`
	- **A1,A2,...Am** = attributes
	- **b0** = **intercept**, its the predicted value of y when all predictors A1, A2,... = 0 (where the regression line crosses y-axis)
	- **b1,b2,...,bm​** = the **slopes (weights)** of each independent variable. Each bi shows how much y is expected to change **when Ai increases by 1 unit** while keeping all other variables constant
		- example, when a1 increases by 1, target (y) increases (go up) by 31.736 (**the model’s predicted output value increases**.)
- **How to obtain regression coefficients?** (slope, intercept)
	- Among many possible lines, choose the one with the **smallest sum of squared (vertical) deviation** (lowest training error)

example 1, **predictor X and numeric target Y**
- training data = (x1,y1),(x2,y2),...,(xn,yn)

![[Pasted image 20251112233337.png]]
**How to we solve for slope and intercept?** : ==Least squares formula==
- Using this formula is better and faster than trying all lines out, cuz this formula already gives us the one line that minimizes error
![[Pasted image 20251112233428.png]]
It measures **how much Y changes when X changes**, based on your data.
- First compute how much **X and Y move together** (covariance)
	- If X increases and Y tends to increase → numerator is **positive** → slope is **positive**
	- For every +1 increase in X, we predict Y increases/decreases by about .....
- Then divide by how much **X varies by itself** (variance of X).
	- If X barely changes, you cannot fit a meaningful line → denominator is small.
- That gives the **best-fit slope**.
- Then compute the intercept so the line crosses the mean point.
- Once we get the regression model, we use it to predict new values, **compute residuals,** SSE,MSE,RMSE

- **Feature selection** can be applied during the **model fitting**
	- Collinear attributes can be removed (attributes that give almost the same info)
- **Residual (error)** for each data point, **error = y - prediction(y)**
- **T-Stat and P-value** = statistical test on coefficient -> to confirm the significant of each attribute
	- P-value close to 0 = **high significance**
- **Tolerance** = indicates **collinearity** between each attribute and the others
	- Should be close to 1
	- Tolerance < 0.2 -> that attribute has **severe collinearity with some others**
### Evaluating Regression Performance
- **Prediction error** `ei = yi - yi hat`
	- **Root of mean squared error (RMSE)**
		- Average error
		- Most common metric

![[Pasted image 20251113000115.png]]

- **Root of relative squared error (RRSE)**
	- Error with respect to **variance** of this data set
	- Normalized RMSE
	- Useful for comparing different models
	- It compares our model’s RMSE to the RMSE of a **baseline model** that always predicts the mean of the target values.
	- **Does our model predict better than the mean?** if RRSE = 0 means perfect prediction

![[Pasted image 20251113000341.png]]
`0 <= RRSE <= 1`

- **Correlation**
	- **strength and direction of a linear relationship** between **two variables** (only consider 2 features at a time)
	- Multiple correlation **between all attributes and Y** (0<=**r**<=1)
		- r = +1 means perfect positive linear relationship
		- r = 0 means no linear relationship
	- Multiple coefficient of **determination** (0<=**R^2**<=1)
		- Tell how much variation in Y can be explained by the model
		- or how well the regression model fits the data
- **Collinearity** ***
	- when **one feature can be linearly predicted from one or more other features** with a high degree of accuracy
	- Any 2 attributes in the model are highly correlated or **having linear relationship with each other******
		- Not good for explaination. when 2 attributes change together, we may get **wrong conclusion** about the effect of each attribute on the target
		- Cannot hold one attribute constant in order to control the effect of the other -> **result in unreliable model**, cuz while increasing A1, A2 also changes whenever A1 does so the model cannot tell which variable(feature) actually caused the change in y
	- Multicollinearity means there are **>2 highly correlated attributes**
	- Example : **A₃ = 2A₁ + 3A₂+10**
		- Meaning, A₃ is a **deterministic linear combination** of A₁ and A₂
		- then A₃ has **no new information**, It is just a combination of A₁ and A₂.

![[Pasted image 20251113001930.png]]

- We use feature selection to **eliminate features with collinearity**
## Extensions of Classification Methods
- [CART](#cart)
- [SVR](#svr-:-support-vector-regression)
### CART
**Classification and Regression Tree**
CART outputs a decision tree where **leaf nodes are either classes** (classification) OR **numeric values** (regression), depending on the type of target variable Y

When target variable Y is numeric, CART builds a tree that **splits the dataset into regions** so that **each region contains data with similar Y values**
- **Least-square error splitting criterion**
	- how CART decides where to split the data (at X > 10, X > 12, etc)
	- for each possible split (feature + split value), CART calculates 
		- **SSE = (y-y hat)squared**, where y hat is the prediction for that region
	For each candidate split (eg. X > 10):
	1. The data is divided into **left region** and **right region**
	2. For each region, the prediction is the **mean y** of that region
		- Calculate the **mean of actual y** in that region in order to get **"prediction y" OF that region**
	3. CART computes the **SSE for left + right**
		- SSE left/right = (y1 - mean y) squared +(y2 - mean y) squared + ...
		- get **total SSE** from left and right region
	4. CART repeats this for **every possible split** and picks the one with the **lowest total SSE** from left and right.
- **Average y in leaf node**
	- Once the tree is grown and you reach a **leaf node**, the prediction for that leaf is
		- **"Mean of all yi in that leaf"**
	- **Why average?** : Because for regression, the value that minimizes squared error for a set of data points is the **mean**.
	- Every new data point that falls into this leaf is predicted as the mean of yi in that leaf
### SVR : Support Vector Regression
Linear regress : fit Y = f(X) that minimizes squared error
SVR = **fit a hyperplane** that keeps points **within** an acceptable margin **ε**

![[Pasted image 20251206124658.png]]

Not all points can be kept inside a margin (ε). Some are allowed to be outside the margin WITH some penalty -> we have to **MINIMIZE total penalty**
- Like SVM, penalty term is controlled by parameter C
- Big C = high penalty, smaller margin. Small C = low penalty
- BUT...why don't we just maximize the margin?
	- Because the ε-tube is **not a margin between classes**, it is an "**acceptable error tolerance**" and not something to optimize
	- the margin is used to ignore small errors (inside ε) and penalize big errors (outside ε)
	- if margin is maximize, the model becomes a flat line (function does not change with x, no slope)
	- Smaller margin = model fits tightly
- After we get ε and C, we when produce slope and intercept to get regression line

SVR can support nonlinear data via **kernel trick**
- nonlinear = line is **not a straight line anymore**
Instead of fitting the model in the original input space, SVR:
1.  **Maps the data into a higher-dimensional space**  
2. Fits a linear SVR in that high-dimensional space**
3. That linear model corresponds to a **nonlinear curve** in the original space.