# Neural Network, SVM
## Neural Network
- normalization, type conversion

### Single Layer perceptron
- Suppose we have 3 attributes (x1,x2,x3) and class y
	- architecture is 3 input nodes + 1 output node (all of these = ===perceptron==)
	- Links between input nodes and output nodes **have different weights** (weights are usually randomed in the beginning)
	- **Input nodes read each training record (x1,x2,x3)** and pass the values to output nodes -> no computation in input layer
	
**How a Perceptron looks like**
![[Pasted image 20251028212216.png]]

- it consists of a single layer (input layer), with multiple neurons (nodes) with their own weights, there are no hidden layers
- It learns the weights for the input signals in order to draw a linear decision boundary
- However, to **solve more complicated, non-linear problems** related to image processing, computer vision, and natural language processing tasks, we work with **deep neural networks.**
### Training a perceptron
- Initialize weights with random values
- For each training cycle
	- For **each training record with known class label y**
		1. Feed training record to the perceptron to predict Y hat
		2. Calculate error = y - y hat
		3. Adjust each weight (back propagation)
	- Complete 1 cycle (all training records are processed) -> Calculate training error
- loop until stopping criteria (weights are stable, training error < threshold) 
### Multilayer perceptron (MLP)
- use **nonlinear activation functions**, which allows the network to **learn complex patterns in data**
- can learn nonlinear relationships in data, making them powerful models for tasks such as classification, regression and pattern recognition
- ==input layer== : consists of nodes or neurons that receive the initial input data. **Each node represents a feature or dimension of the input data**. The number of neurons in the input layer is determined by the dimensionality of the input data.
- ==hidden layer == receives inputs from all neurons in the previous layer and produces an output that is passed to the next layer. 1 layer is enough for most classification tasks. 
	- Too many layers give worse performance because having many backpropagation, the gradients gets multiplied **many times** by small (or large) numbers — because each layer contributes a derivative. Weights can shrink so that it stops learning, or it can explode. So early layers dont learn useful features. **Also leads to overfitting for having more parameters**. 
	- **Number of nodes in each layer**
		- Between the sizes of input and output layers
		- Typical size = (# input nodes + # output nodes) / 2
- ==output layer== :  produce the final output of the network. (binary classification or multi-class classification)
- ==weights== : Nodes in adjacent layers are fully connected to each other. **Each connection has an associted weight, which determines the strength of the connection**. These weights are LEARNED DURING the TRAINING process
- ==Bias neurons== : In addition to the input and hidden neurons, **each layer** (except the input layer) usually **includes a bias neuron** that **provides a constant input to the neurons in the next layer.** Bias neurons **have their own weight** associated with each connection, which is also learned during training.
	- It effectively **shifts the activation function of the neurons** in the subsequent layer, allowing the network to learn an offset or bias in the decision boundary. By adjusting the weights connected to the bias neuron, the MLP can learn to **control the threshold for activation** and **better fit** the training data.
- ==Back Propagation Training== 
	- we dont know output from hidden layer
	- To process each record 
		- **Forward phase** -> feed input values through MLP until we get final prediction. The final **output is** **compared to the true label** to compute the **loss**
		- **Backward phase** -> The algorithm computes **gradients**, how much the loss changes with respect to each parameter (each weight and bias). It uses the **chain rule** from calculus to propagate errors **from the output layer back to earlier layers**.
	- Once gradients are computed, the network updates each parameter (weight and bias) to minimize the loss. **So weights are adjusted from back to front**
	- **Weight adjustment** (during back propagation): is based on **gradient descent concept**
		- Find slope/**gradient** of current point with respect to **each weight dimension.** (slope of Loss & Weight)
			- Gradient tells how much **loss** would change **if we tweak that weight slightly**. If gradient/slope is positive -> we are on an upward direction -> we move downward (subtract the gradient)
		- Walk along that direction **toward the point where error is minimum**
	![[image_bd3b978959.avif]]
### Node processing
- summation, activiation (Sigmoid)
==** every neuron has an activation function**==

**Output node / perceptron has 2 functions**
![[Pasted image 20251028221001.png]]

**Suppose that** 
- weight vector : W = (w1,w2,w3) = (0.3,0.3,0.3)
- Bias = 0.4
- Sum = 0.3x1 + 0.3x2 + 0.3x3 - 0.4
- Activation function = sign function where ![[Pasted image 20251028222045.png]]

**Consider training records (x1,x2,x3,y)**, where y is label (actual class) and x1,x2,x3 are categories
- (1,0,0,-1) -> y hat = -1 Correct prediction
- (1,0,1,-1) -> y hat = 1 wrong prediction
- (1,1,0,1) -> y hat = 1 Correct prediction
#### Activation Function (sigmoid)
uses to produce the output
![[Pasted image 20251028222526.png]]

![[Pasted image 20251028224611.png]]

- real number x (or z) is between 0 and 1, which is perfect for interpreting as a probabilty
- So, the **summation function** replaces z in order to compute the value from this "activation function" (summation function is `w1​x1​+w2​x2+.... ​−b`, which is the **weighted sum of inputs**)
- ==INPUT = Summation function==
**Bias**
- the bias (-b) **SHIFTS the summation** by subtracting the threshold b
- If b is **large**, we need **larger input** to make z positive → neuron activates **less easily**.
- If b is **small**, z becomes positive more easily → neuron activates **more easily**
- b shifts the whole curve to left & right, shifts right when b increase (high bias, harder to activate neuron, meaning **the neuron now requires larger input (x-axis) to reach the same activation level**)
- larger input value = stronger total signal coming into the neuron (more weight or inputs xi)
**How to make neuron activates easily?**
- Increasing **weights** (wi​) → increases z → **higher output** → neuron **activates more strongly**. (Stronger influence from input)
- Increasing **bias/threshold** (b) → shifts curve right → requires bigger z → **harder to activate**.
- Neuron output is **between 0-1**, the activation is "continuous"
### Attributes
- All considered together by weight adjustment
### Parameters : Learning rate, momentum
#### Learning Rate (n)
0 < n < 1
- Controls the **amount of each weight adjustment**
- controls **how big each step** is when we move downhill on the loss curve.
- too small n : new weight is close to old weight, slow convergence
- too big n : new weight is sensitive to the current prediction, each step becomes **so big** that instead of stopping near the bottom, we jump **past it** all the way to the other side of the valley (oscillation of weight values or overshooting optimal (best) point)
- Oscillation
	- Each time we cross the minimum loss point, the slope flips sign "horizontally" → the next update goes the other way → back and forth.
- Smooth hill
	- height = error model has (loss)
	- horizontal position = value of weight w
- Solution
	- Assign large n in the first few training cycles, so weights are quickly adjusted to near optimal point
	- Decrease n in subsequent cycles, so weights are fine-tuned to each optimal point
#### Momentum
- 0 < alpha < 1
- **Smoothen weight adjustment** (helps training move faster and smoothly by remembering previous updates)
- By keeping the momentum from previous adjustment -> so that the current adjustment doesn't sharply swing to the opposite direction
- If you’ve been moving in the same direction for a while, momentum makes you go **faster** that way. If gradients start to zigzag, momentum helps **smooth out** the motion so you don’t bounce too wildly.
- **When are Gradients zigzag?** = we have many weights, high-dimensional
- **Too small** : not enough momentum to reach global optimum (stuck at local optimum). It forgets past gradients quickly. The optimizer reacts sharply to small slope changes.
	- Effect : still zigzags in steep valleys (momentum not strong enough to smooth it out)
- **Too big** : Overshooting global optimum. When the direction changes, it’s **slow to respond** and tends to **overshoot** the minimum DUE to high speed
## SVM 
find an optimal linear hyperplane (decision boundary) that **seperate data into correct classes**
Objective = **find decision boundary with max margin**
### hyperplane 
- binary classification : separate data into 2 sides of hyperplane
- Margin should be as big as possible -> perform better on unseen data
	- small margins can be overfitting
- ==Support vectors== : data points close to hyperplane that **influence the position & orientation of the plane** 
- SVM chooses the hyperplane that maximize the margin (the distance between the **closest points of both classes**) the closet points are "support vectors"
- Pptimal hyperplane must have **minimum total penalty**
	- (The margin is still as **wide** as possible while keeping errors **minimal** and Fewer points violate the margin or are misclassified)
	- if constraints are not completely satisfied -> minimize training error/**total penalty (C)**
		- Big C = high penalty for misprediction, **narrow margin to avoid misprediction**, may result in overfitting
		- Small C = low penalty for misprediction, **wide margin to allow some misprediction**, but may result in underfitting

**Example : setting a decision boundary**
- let data D consists of **tuple (xi, yi)** where yi is class label being positive (+1) or negative (-1)
- **decision boundary W * X + b = 0 has 2 parameters**
	- W = weight vector (w1,w2,...,wn); n = number of attributes (like age, salary, experience)
	- X is a vector that contains **all the attributes (features)** for one data sample.
	- b = bias
- Therefore, the **prediction y for any data point x** :
![[Pasted image 20251029002856.png]]

![[Pasted image 20251029003154.png]]

- parallel hyperlanes above & below decision boundary (**Constraints**)
- b upper = W * X + b = 1
- b lower = W * X + b = -1

![[Pasted image 20251029003300.png]]

### kernel function (dot, polynomial)
- **Problem** : What if data isn’t linearly separable? (meaning, cannot be separated by a hyperplane)
SVM by default, find linear boundaries. but if the data is nonlinear, we need Kernel trick
- Solution : map data to a higher dimension (so that the classes can become linearly separable)
- **Another problem** : In multidimensional, its difficult to find ![[Pasted image 20251029005241.png]]
- solving optimization may be even more difficult and computationally expensive
**So heres kernel trick** : 
Instead of explicitly mapping the data into higher dimensions,  
the **kernel function** lets us compute the _dot product in that high-dimensional space_  
**without ever doing the transformation**
- Consider data points **U = (u1,u2,...,un) and V = (v1,v2,...,vn)**, which are vectors in feature space (**each data point** is represented by **a vector of its attributes**)
	- U dot V = cosine similarity = angle between 2 vectors
	- ![[Pasted image 20251029010011.png]] = **the same angle between 2 vectors**
- If we have a kernel function ![[Pasted image 20251029010139.png]] -> dot product in a new space **can be computed using values in the original space** and we **DONT CARE ABT** ![[Pasted image 20251029010208.png]]

So, instead of calculating ![[Pasted image 20251029010258.png]]
We can use Kernel ![[Pasted image 20251029010326.png]]

This is like **transforming data from original space (non linear) to a new space (linear)**
#### dot
The **dot product** between two feature vectors measures **similarity**
![[Pasted image 20251029010421.png]]
- if the 2 vectors point in the same direction (similar data points) -> dot product is large (high similarity)
- if they point in opposite direction (different classes) -> product is small or negative (low similarity)
#### polynomial degree d
![[Pasted image 20251029010428.png]]
The **degree d** in the polynomial kernel controls **how complex the decision boundary is** 
higher d = more nonlinear features, more flexible curve, but also more risk of overfitting.
- degree 1 : effect -> linear kernel -> straight line
- degree 2 : effect -> adds pairwise feature interactions -> parabola,curve
- Dot product or polynomial (degree 1 or 2) is recommended for simple classification

