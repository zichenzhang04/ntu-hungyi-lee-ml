The course website: [MACHINE LEARNING 2022 SPRING](https://speech.ee.ntu.edu.tw/~hylee/ml/2022-spring.php)

# 2/18 Lecture 1: Introduction of Deep Learning

## Preparation 1: 預測本頻道觀看人數 (上)-機器學習基本概念簡介

ML is about finding a function.

Besides **regression** and **classification** tasks, ML also focuses on **<u>Structured Learning</u>** -- create something with structure (image, document).

Three main ML tasks:

- Regression
- Classification
- Structured Learning

### Function with unknown parameters

*Guess* a function (**model**) $y=b+wx_1$ (in this case, a ***linear model***) based on **domain knowledge**. $w$ and $b$ are unknown parameters. 

### Define Loss From Training Data

Loss is a function itself of parameters, written as $L(b,w)$​. 

An example loss function is:

$$
L = \frac{1}{N} \sum_{n} e_n
$$

where $e_n$ can be calculated in many ways:

**Mean Absolute Error (MAE)**

$$
e_n = |y_n - \hat{y}_n|
$$

**Mean Square Error (MSE)**

$$
e_n = (y_n - \hat{y}_n)^2
$$

If $y$ and $\hat{y}_n$ are both probability distributions, we can use **cross entropy**.

<img src="assets/image-20240503102744604.png" alt="image-20240503102744604" style="zoom:25%;" />

### Optimization

The goal is to find the best parameter:

$$
w^{*}, b^{*} = \arg\min_{w,b} L
$$

Solution: **Gradient Descent**

1. Randomly pick an intitial value $w^0$
2. Compute $\frac{\partial L}{\partial w} \bigg|_{w=w^0}$ (if it's negative, we should increase $w$; if it's positive, we should decrease $w$​)
3. Perform update step: $w^1 \leftarrow w^0 - \eta \frac{\partial L}{\partial w} \bigg|_{w=w^0}$ iteratively
4. Stop either when we've performed a maximum number of update steps or the update stops ($\eta \frac{\partial L}{\partial w} = 0$)

If we have two parameters:

1. (Randomly) Pick initial values $w^0, b^0$

2. Compute:

   $w^1 \leftarrow w^0 - \eta \frac{\partial L}{\partial w} \bigg|_{w=w^0, b=b^0}$

   $b^1 \leftarrow b^0 - \eta \frac{\partial L}{\partial b} \bigg|_{w=w^0, b=b^0}$​

<img src="assets/image-20240503111251767.png" alt="image-20240503111251767" style="zoom:25%;" />

## Preparation 2: 預測本頻道觀看人數 (下)-機器學習基本概念簡介

### Beyond Linear Curves: Neural Networks

However, linear models are too simple. Linear models have severe limitations called **model bias**.

<img src="assets/image-20240503112224217.png" alt="image-20240503112224217" style="zoom:25%;" />

All piecewise linear curves:

<img src="assets/image-20240503123546349.png" alt="image-20240503123546349" style="zoom:25%;" />

More pieces require more blue curves.

<img src="assets/image-20240503123753582.png" alt="image-20240503123753582" style="zoom:25%;" />

How to represent a blue curve (**Hard Sigmoid** function): **Sigmoid** function

$$
y = c\:\frac{1}{1 + e^{-(b + wx_1)}} = c\:\text{sigmoid}(b + wx_1)
$$

We can change $w$, $c$ and $b$ to get sigmoid curves with different shapes.

Different Sigmoid curves -> Combine to approximate different piecewise linear functions -> Approximate different continuous functions

<img src="assets/image-20240503124800696.png" alt="image-20240503124800696" style="zoom: 25%;" />

<img src="assets/image-20240503125224141.png" alt="image-20240503125224141" style="zoom: 25%;" />

From a model with high bias $y=b+wx_1$ to the new model with more features and a much lower bias:

$$
y = b + \sum_{i} \; c_i \; \text{sigmoid}(b_i + w_i x_1)
$$

Also, if we consider multiple features $y = b + \sum_{j} w_j x_j$​, the new model can be expanded to look like this:

$$
y = b + \sum_{i} c_i \; \text{sigmoid}(b_i + \sum_{j} w_{ij} x_j)
$$

Here, $i$ represents each sigmoid function and $j$ represents each feature. $w_{ij}$ represents the weight for $x_j$ that corresponds to the $j$-th feature in the $i$-th sigmoid.

<img src="assets/image-20240503131652987.png" alt="image-20240503131652987" style="zoom:33%;" />

<img src="assets/image-20240503132233638.png" alt="image-20240503132233638" style="zoom: 25%;" />

<img src="assets/image-20240503132324232.png" alt="image-20240503132324232" style="zoom: 25%;" />

<img src="assets/image-20240503132402405.png" alt="image-20240503132402405" style="zoom:25%;" />
$$
y = b + \boldsymbol{c}^T \sigma(\boldsymbol{b} + W \boldsymbol{x})
$$

$\boldsymbol{\theta}=[\theta_1, \theta_2, \theta_3, ...]^T$ is our parameter vector:

<img src="assets/image-20240503132744112.png" alt="image-20240503132744112" style="zoom: 25%;" />

Our loss function is now expressed as $L(\boldsymbol{\theta})$.

<img src="assets/image-20240503134032953.png" alt="image-20240503134032953" style="zoom:25%;" />

Optimization is still the same. 

$$
\boldsymbol{\theta}^* = \arg \min_{\boldsymbol{\theta}} L
$$

1. (Randomly) pick initial values $\boldsymbol{\theta}^0$​
2. calculate the gradient $\bold{g} = \begin{bmatrix} \frac{\partial{L}}{\partial{\theta_1}}\bigg|_{\boldsymbol{\theta}=\boldsymbol{\theta}^0} \\ \frac{\partial{L}}{\partial{\theta_2}}\bigg|_{\boldsymbol{\theta}=\boldsymbol{\theta}^0} \\ \vdots \end{bmatrix} = \nabla L(\boldsymbol{\theta}^0)$ with $\boldsymbol{\theta}, \boldsymbol{g} \in \mathbb{R}^n$
3. perform the update step: $\begin{bmatrix} \theta_1^1 \\ \theta_2^1 \\ \vdots \end{bmatrix} \leftarrow \begin{bmatrix} \theta_1^0 \\ \theta_2^0 \\ \vdots \end{bmatrix} - \begin{bmatrix} \eta \frac{\partial{L}}{\partial{\theta_1}}\bigg|_{\boldsymbol{\theta}=\boldsymbol{\theta}^0} \\ \eta \frac{\partial{L}}{\partial{\theta_2}}\bigg|_{\boldsymbol{\theta}=\boldsymbol{\theta}^0} \\ \vdots \end{bmatrix}$, namely $\boldsymbol{\theta}^1 \leftarrow \boldsymbol{\theta}^0 - \eta \boldsymbol{g}$

The terms ***batch*** and ***epoch*** are different.

<img src="assets/image-20240503155656244.png" alt="image-20240503155656244" style="zoom:33%;" />

$$
\text{num\_updates} = \frac{\text{num\_examples}}{\text{batch\_size}}
$$

Batch size $B$​ is also a hyperparameter. One epoch does not tell the number of updates the training process actually has.

### More activation functions: RELU

It looks kind of like the Hard Sigmoid function we saw earlier:

<img src="assets/image-20240503161144168.png" alt="image-20240503161144168" style="zoom:25%;" />

As a result, we can use these two RELU curves to simulate the same result as Sigmoid curve.

$$
y = b + \sum_{2i} c_i \max(0, b_i + \sum_j w_{ij}x_j)
$$

<img src="assets/image-20240503162214759.png" alt="image-20240503162214759" style="zoom:25%;" />

*But, why we want "Deep" network, not "Fat" network*? Answer to that will be revealed in a later lecture.

# 2/25 Lecture 2: What to do if my network fails to train

## Preparation 1: 機器學習任務攻略

### Frameworks of ML

Training data is $\{(\boldsymbol{x}^1, \hat{y}^1), (\boldsymbol{x}^2, \hat{y}^2), ...,(\boldsymbol{x}^N, \hat{y}^N)\}$. Testing data is $\{ \boldsymbol{x}^{N+1}, \boldsymbol{x}^{N+2}, ..., \boldsymbol{x}^{N+M} \}$​

Traing steps:

1. write a function with unknown parameters, namely $y = f_{\boldsymbol{\theta}}(\boldsymbol{x})$​
2. define loss from training data: $L(\boldsymbol{\theta})$
3. optimization: $\boldsymbol{\theta}^* = \arg \min_{\boldsymbol{\theta}} L$​
4. Use $y = f_{\boldsymbol{\theta}^*}(\boldsymbol{x})$​ to label the testing data

### General Guide

<img src="assets/image-20240504115031073.png" alt="image-20240504115031073" style="zoom: 25%;" />

### Potential issues during training

Model bias: the potential function set of our model does not even include the desired optimal function/model.

<img src="assets/image-20240503173246516.png" alt="image-20240503173246516" style="zoom: 25%;" />

Large loss doesn't always imply issues with model bias. There may be issues with *optimization*. That is, gradient descent does not always produce global minima. We may stuck at a local minima. In the language of function set, the set theoretically contain optimal function $f^*(\boldsymbol{x})$. However, we may never reach that.

<img src="assets/image-20240503174333051.png" alt="image-20240503174333051" style="zoom: 33%;" />

### Optimization issue

- gain insights from comparison to identify whether the failure of the model is due to optimization issues, overfitting or model bias
- start from shallower networks (or other models like SVM which is much easier to optimize)
- if deeper networks do not contain smaller loss on *training* data,  then there is optimization issues (as seen from the graph below)

<img src="assets/image-20240504100720536.png" alt="image-20240504100720536" style="zoom:25%;" />

For example, here, the 5-layer should always do better or the same as the 4-layer network. This is clearly due to optimization problems.

<img src="assets/image-20240504100948916.png" alt="image-20240504100948916" style="zoom:25%;" />

### Overfitting

Solutions:

- more training data
- data augumentation (in image recognition, it means flipping images or zooming in images)
- use a more constrained model: 
  - less parameters
  - sharing parameters
  - less features
  - early stopping
  - regularization
  - dropout

For example, CNN is a more constrained version of the fully-connected vanilla neural network.

<img src="assets/image-20240504102902784.png" alt="image-20240504102902784" style="zoom:25%;" />

### Bias-Complexity Trade-off

<img src="assets/image-20240504104404308.png" alt="image-20240504104404308" style="zoom:25%;" />

<img src="assets/image-20240504104623658.png" alt="image-20240504104623658" style="zoom:25%;" />

### N-fold Cross Validation

<img src="assets/image-20240504114823462.png" alt="image-20240504114823462" style="zoom:25%;" />

### Mismatch

Mismatch occurs when the training dataset and the testing dataset comes from different distributions. Mismatch can not be prevented by simply increasing the training dataset like we did to overfitting. More information on mismatch will be provided in Homework 11.

## Preparation 2: 類神經網路訓練不起來怎麼辦 (一)： 局部最小值 (local minima) 與鞍點 (saddle point)

### Optimization Failure

<img src="assets/image-20240504115754108.png" alt="image-20240504115754108" style="zoom:25%;" />

<img src="assets/image-20240504115906505.png" alt="image-20240504115906505" style="zoom:25%;" />

Optimization fails not always because of we stuck at local minima. We may also encounter **saddle points**, which are not local minima but have a gradient of $0$. 

All the points that have a gradient of $0$ are called **critical points**. So, we can't say that our gradient descent algorithms always stops because we stuck at local minima -- we may stuck at saddle points as well. The correct way to say is that gradient descent stops when we stuck at a critical point.

If we are stuck at a local minima, then there's no way to further decrease the loss (all the points around local minima are higher); if we are stuck at a saddle point, we can escape the saddle point. But, how can we differentiate a saddle point and local minima?

### Identify which kinds of Critical Points

$L(\boldsymbol{\theta})$ around $\boldsymbol{\theta} = \boldsymbol{\theta}'$ can be approximated (Taylor Series)below:

$$
L(\boldsymbol{\theta}) \approx L(\boldsymbol{\theta}') + (\boldsymbol{\theta} - \boldsymbol{\theta}')^T \boldsymbol{g} + \frac{1}{2} (\boldsymbol{\theta} - \boldsymbol{\theta}')^T H (\boldsymbol{\theta} - \boldsymbol{\theta}')
$$

Gradient $\boldsymbol{g}$ is a *vector*:

$$
\boldsymbol{g} = \nabla L(\boldsymbol{\theta}')
$$

$$
\boldsymbol{g}_i = \frac{\partial L(\boldsymbol{\theta}')}{\partial \boldsymbol{\theta}_i}
$$

Hessian $H$ is a matrix:

$$
H_{ij} = \frac{\partial^2}{\partial \boldsymbol{\theta}_i \partial \boldsymbol{\theta}_j} L(\boldsymbol{\theta}')
$$

<img src="assets/image-20240504121739774.png" alt="image-20240504121739774" style="zoom:25%;" />

The green part is the Gradient and the red part is the Hessian.

When we are at the critical point, The approximation is "dominated" by the Hessian term.

<img src="assets/image-20240504122010303.png" alt="image-20240504122010303" style="zoom:25%;" />

Namely, our approximation formula becomes:

$$
L(\boldsymbol{\theta}) \approx L(\boldsymbol{\theta}') + \frac{1}{2} (\boldsymbol{\theta} - \boldsymbol{\theta}')^T H (\boldsymbol{\theta} - \boldsymbol{\theta}') = L(\boldsymbol{\theta}') + \frac{1}{2} \boldsymbol{v}^T H \boldsymbol{v}
$$

Local minima: 

- For all $\boldsymbol{v}$, if $\boldsymbol{v}^T H \boldsymbol{v} > 0$ ($H$ is positive definite, so all eigenvalues are positive), around $\boldsymbol{\theta}'$: $L(\boldsymbol{\theta}) > L(\boldsymbol{\theta}')$​

Local maxima:

- For all $\boldsymbol{v}$, if $\boldsymbol{v}^T H \boldsymbol{v} < 0$ ($H$ is negative definite, so all eigenvalues are negative), around $\boldsymbol{\theta}'$: $L(\boldsymbol{\theta}) < L(\boldsymbol{\theta}')$

Saddle point:

- Sometimes $\boldsymbol{v}^T H \boldsymbol{v} < 0$, sometimes $\boldsymbol{v}^T H \boldsymbol{v} > 0$. Namely, $H$​ is indefinite -- some eigenvalues are positive and some eigenvalues are negative.

Example:

<img src="assets/image-20240504130115656.png" alt="image-20240504130115656" style="zoom:25%;" />

<img src="assets/image-20240504130345730.png" alt="image-20240504130345730" style="zoom:33%;" />

### Escaping saddle point

If by analyzing $H$'s properpty, we realize that it's indefinite (we are at a saddle point). We can also analyze $H$ to get a sense of the **parameter update direction**! 

Suppose $\boldsymbol{u}$ is an eigenvector of $H$ and $\lambda$ is the eigenvalue of $\boldsymbol{u}$.

$$
\boldsymbol{u}^T H \boldsymbol{u} = \boldsymbol{u}^T (H \boldsymbol{u}) = \boldsymbol{u}^T (\lambda \boldsymbol{u}) = \lambda (\boldsymbol{u}^T \boldsymbol{u}) = \lambda \|\boldsymbol{u}\|^2
$$

If the eigenvalue $\lambda < 0$, then $\boldsymbol{u}^T H \boldsymbol{u} = \lambda \|\boldsymbol{u}\|^2 < 0$ (eigenvector $\boldsymbol{u}$ can't be $\boldsymbol{0}$). Because $L(\boldsymbol{\theta}) \approx L(\boldsymbol{\theta}') + \frac{1}{2} \boldsymbol{u}^T H \boldsymbol{u}$, we know $L(\boldsymbol{\theta}) < L(\boldsymbol{\theta}')$. By definition, $\boldsymbol{\theta} - \boldsymbol{\theta}' = \boldsymbol{u}$. If we perform $\boldsymbol{\theta} = \boldsymbol{\theta}' + \boldsymbol{u}$, we can effectively decrease $L$. We can escape the saddle point and decrease the loss.

However, this method is seldom used in practice because of the huge computation need to compute the Hessian matrix and the eigenvectors/eigenvalues.

### Local minima v.s. saddle point

<img src="assets/image-20240504143925130.png" alt="image-20240504143925130" style="zoom:25%;" />

A local minima in lower-dimensional space may be a saddle point in a higher-dimension space. Empirically, when we have lots of parameters, **local minima is very rare**. 

<img src="assets/image-20240504144357680.png" alt="image-20240504144357680" style="zoom:25%;" />

## Preparation 3: 類神經網路訓練不起來怎麼辦 (二)： 批次 (batch) 與動量 (momentum)

### Small Batch v.s. Large Batch

<img src="assets/image-20240504145118594.png" alt="image-20240504145118594" style="zoom:25%;" />

<img src="assets/image-20240504145348568.png" alt="image-20240504145348568" style="zoom:25%;" />

Note that here, "time for cooldown" does not always determine the time it takes to complete an epoch.

Emprically, large batch size $B$​ does **not** require longer time to compute gradient because of GPU's parallel computing, unless the batch size is too big.

<img src="assets/image-20240504145814623.png" alt="image-20240504145814623" style="zoom:25%;" />

**Smaller** batch requires **longer** time for <u>one epoch</u> (longer time for seeing all data once).

<img src="assets/image-20240504150139906.png" alt="image-20240504150139906" style="zoom:33%;" />

However, large batches are not always better than small batches. That is, the noise brought by small batches lead to better performance (optimization). 

<img src="assets/image-20240504152856857.png" alt="image-20240504152856857" style="zoom:33%;" />

<img src="assets/image-20240504151712866.png" alt="image-20240504151712866" style="zoom: 33%;" />

Small batch is also better on **testing** data (***overfitting***).

![image-20240504154312760](assets/image-20240504154312760.png)

This may be because that large batch is more likely to lead to us stucking at a **sharp minima**, which is not good for testing loss. Because of noises, small batch is more likely to help us escape sharp minima. Instead, at convergence, we will more likely end up in a **flat minima**.

<img src="assets/image-20240504154631454.png" alt="image-20240504154631454" style="zoom: 25%;" />

Batch size is another hyperparameter.

<img src="assets/image-20240504154938533.png" alt="image-20240504154938533" style="zoom:25%;" />

### Momentum

Vanilla Gradient Descent:

<img src="assets/image-20240504160206707.png" alt="image-20240504160206707" style="zoom:25%;" />

Gradient Descent with Momentum:

<img src="assets/image-20240504160436876.png" alt="image-20240504160436876" style="zoom: 25%;" />

<img src="assets/image-20240504160549105.png" alt="image-20240504160549105" style="zoom:25%;" />

### Concluding Remarks

<img src="assets/image-20240504160755845.png" alt="image-20240504160755845" style="zoom:33%;" />

## Preparation 4: 類神經網路訓練不起來怎麼辦 (三)：自動調整學習速率 (Learning Rate)

### Problems with Gradient Descent

The fact that training process is stuck does not always mean small gradient.

<img src="assets/image-20240504161124291.png" alt="image-20240504161124291" style="zoom:33%;" />

Training can be difficult even without critical points. Gradient descent can fail to send us to the global minima even under the circumstance of a **convex** error surface. You can't fix this problem by adjusting the learning rate $\eta$.

<img src="assets/image-20240504161746667.png" alt="image-20240504161746667" style="zoom:33%;" />

Learning rate can not be one-size-fits-all. **If we are at a place where the gradient is high (steep surface), we expect $\eta$ to be small so that we don't overstep; if we are at a place where the gradient is small (flat surface), we expect $\eta$ to be large so that we don't get stuck at one place.**

<img src="assets/image-20240504162232403.png" alt="image-20240504162232403" style="zoom:25%;" />

### Adagrad

Formulation for one parameter:

$$
\boldsymbol{\theta}_i^{t+1} \leftarrow \boldsymbol{\theta}_i^{t} - \eta \boldsymbol{g}_i^t
$$

$$
\boldsymbol{g}_i^t = \frac{\partial L}{\partial \boldsymbol{\theta}_i} \bigg |_{\boldsymbol{\theta} = \boldsymbol{\theta}^t}
$$

The new formulation becomes:

$$
\boldsymbol{\theta}_i^{t+1} \leftarrow \boldsymbol{\theta}_i^{t} - \frac{\eta}{\sigma_i^t} \boldsymbol{g}_i^t
$$

$\sigma_i^t$ is both parameter-dependent ($i$) and iteration-dependent ($t$). It is called **Root Mean Square**. It is used in **Adagrad** algorithm.

$$
\sigma_i^t = \sqrt{\frac{1}{t+1} \sum_{i=0}^t (\boldsymbol{g}_i^t)^2}
$$
<img src="assets/image-20240504212350040.png" alt="image-20240504212350040" style="zoom:25%;" />

Why this formulation works?

<img src="assets/image-20240504212744865.png" alt="image-20240504212744865" style="zoom:25%;" />

### RMSProp

However, this formulation still has some problems. We assumed that the gradient for one parameter will stay relatively the same. However, it's not always the case. For example, there may be places where the gradient becomes large and places where the gradient becomes small (as seen from the graph below). The reaction of this formulation to a new gradient change is very slow.

<img src="assets/image-20240504213324493.png" alt="image-20240504213324493" style="zoom:25%;" />

The new formulation is now:

$$
\sigma_i^t = \sqrt{\alpha(\sigma_i^{t-1})^2 + (1-\alpha)(\boldsymbol{g}_i^t)^2}
$$
$\alpha$ is a hyperparameter ($0 < \alpha < 1$). It controls how important the previously-calculated gradient is.

<img src="assets/image-20240504214302296.png" alt="image-20240504214302296" style="zoom:25%;" />

<img src="assets/image-20240504214445048.png" alt="image-20240504214445048" style="zoom:25%;" />

### Adam

The Adam optimizer is basically the combination of RMSProp and Momentum.

![image-20240504214928718](assets/image-20240504214928718.png)

### Learning Rate Scheduling

This is the optimization process with Adagrad:

<img src="assets/image-20240504215606600.png" alt="image-20240504215606600" style="zoom:33%;" />

To prevent the osciallations at the final stage, we can use two methods:

$$
\boldsymbol{\theta}_i^{t+1} \leftarrow \boldsymbol{\theta}_i^{t} - \frac{\eta^t}{\sigma_i^t} \boldsymbol{g}_i^t
$$

#### Learning Rate Decay

As the training goes, we are closer to the destination. So, we reduce the learning rate $\eta^t$​.

<img src="assets/image-20240504220358590.png" alt="image-20240504220358590" style="zoom:25%;" />

This improves the previous result:

<img src="assets/image-20240504220244480.png" alt="image-20240504220244480" style="zoom:33%;" />

#### Warm up

<img src="assets/image-20240504220517645.png" alt="image-20240504220517645" style="zoom:25%;" />

We first increase $\eta ^ t$ and then decrease it. This method is used in both the Residual Network and Transformer paper. At the beginning, the estimate of $\sigma_i^t$​​ has large variance. We can learn more about this method in the RAdam paper.

### Summary

<img src="assets/image-20240504222113767.png" alt="image-20240504222113767" style="zoom:25%;" />

## Preparation 5: 類神經網路訓練不起來怎麼辦 (四)：損失函數 (Loss) 也可能有影響

### How to represent classification

We can't directly apply regression to classification problems because regression tends to penalize the examples that are "too correct." 

<img src="assets/image-20240505103637180.png" alt="image-20240505103637180" style="zoom:25%;" />

It's also problematic to directly represent Class 1 as numeric value $1$, Class 2 as $2$, Class 3 as $3$​. That is, this representation has an underlying assumption that Class 1 is "closer" or more "similar" to Class 2 than Class 3. However, this is not always the case.

One possible model is:

$$
f(x) = \begin{cases} 
1 & g(x) > 0 \\
2 & \text{else}
\end{cases} 
$$

The loss function denotes the number of times $f$ gets incorrect results on training data.

$$
L(f) = \sum_n \delta(f(x^n) \neq \hat{y}^n)
$$

We can represent classes as one-hot vectors. For example, we can represent Class $1$ as $\hat{y} = \begin{bmatrix}
1 \\
0 \\
0
\end{bmatrix}$, Class $2$ as $\hat{y} = \begin{bmatrix}
0 \\
1 \\
0
\end{bmatrix}$ and Class $3$ as $\hat{y} = \begin{bmatrix}
0 \\
0 \\
1
\end{bmatrix}$.

<img src="assets/image-20240505084900542.png" alt="image-20240505084900542" style="zoom: 25%;" />

### Softmax

$$
y_i' = \frac{\exp(y_i)}{\sum_j \exp(y_j)}
$$

We know that $0 < y_i' < 1$ and $\sum_i y_i' = 1$.

<img src="assets/image-20240505085254461.png" alt="image-20240505085254461" style="zoom: 33%;" />

<img src="assets/image-20240505085830849.png" alt="image-20240505085830849" style="zoom: 33%;" />

### Loss of Classification

#### Mean Squared Error (MSE)

$$
e = \sum_i (\boldsymbol{\hat{y}}_i - \boldsymbol{y}_i')^2
$$

#### Cross-Entropy

$$
e = -\sum_i \boldsymbol{\hat{y}}_i \ln{\boldsymbol{y}_i'}
$$

Minimizing cross-entropy is equivalent to maximizing likelihood. 

Cross-entropy is more frequently used for classification than MSE. At the region with higher loss, the gradient of MSE is close to $0$. This is not good for gradient descent. 

<img src="assets/image-20240505091600454.png" alt="image-20240505091600454" style="zoom:33%;" />

### Generative Models

<img src="assets/image-20240505110347099.png" alt="image-20240505110347099" style="zoom:25%;" />

$$
P(C_1 \mid x) 
= \frac{P(C_1, x)}{P(x)} 
= \frac{P(x \mid C_1)P(C_1)}{P(x \mid C_1)P(C_1) + P(x \mid C_2)P(C_2)}
$$

We can therefore predict the distribution of $x$:

$$
P(x) = P(x \mid C_1)P(C_1) + P(x \mid C_2)P(C_2)
$$

#### Prior

$P(C_1)$ and $P(C_2)$ are called prior probabilities. 

#### Gaussian distribution

$$
f_{\mu, \Sigma}(x) = \frac{1}{(2\pi)^{D/2} |\Sigma|^{1/2}} \exp\left(-\frac{1}{2} (x - \mu)^T \Sigma^{-1} (x - \mu)\right)
$$

Input: vector $x$, output: probability of sampling $x$. The shape of the function determines by mean $\mu$ and covariance matrix $\Sigma$. ==Technically, the output is the probability density, not exactly the probability, through they are positively correlated.==

<img src="assets/image-20240505111630135.png" alt="image-20240505111630135" style="zoom:25%;" />

<img src="assets/image-20240505111652217.png" alt="image-20240505111652217" style="zoom:25%;" />

#### Maximum Likelihood

We assume $x^1, x^2, x^3, \cdots, x^{79}$ generate from the Gaussian ($\mu^*, \Sigma^*$) with the *maximum likelihood*.

$$
L(\mu, \Sigma) = f_{\mu, \Sigma}(x^1) f_{\mu, \Sigma}(x^2) f_{\mu, \Sigma}(x^3) \cdots f_{\mu, \Sigma}(x^{79})
$$

$$
\mu^*, \Sigma^* = \arg \max_{\mu,\Sigma} L(\mu, \Sigma)
$$

The solution is as follows:

$$
\mu^* = \frac{1}{79} \sum_{n=1}^{79} x^n
$$

$$
\Sigma^* = \frac{1}{79} \sum_{n=1}^{79} (x^n - \mu^*)(x^n - \mu^*)^T
$$

<img src="assets/image-20240505115811655.png" alt="image-20240505115811655" style="zoom:25%;" />

But the above generative model fails to give a high-accuracy result. Why? In that formulation, every class has its unique mean vector and covariance matrix. The size of the covariance matrix tends to increase as the feature size of the input increases. This increases the number of trainable parameters, which tends to result in overfitting. Therefore, we can force different distributions to **share the same covariance matrix**.

<img src="assets/image-20240505120606165.png" alt="image-20240505120606165" style="zoom:25%;" />

<img src="assets/image-20240505121134979.png" alt="image-20240505121134979" style="zoom:25%;" />

Intuitively, the new covariance matrix is the sum of the original covariance matrices weighted by the frequencies of samples in each distribution.

<img src="assets/image-20240505121610514.png" alt="image-20240505121610514" style="zoom: 33%;" />

<img src="assets/image-20240505122313657.png" alt="image-20240505122313657" style="zoom: 25%;" />

#### Three steps to a probability distribution model

<img src="assets/image-20240505123718777.png" alt="image-20240505123718777" style="zoom: 25%;" />

We can always use whatever distribution we like (we use Guassian in the previous example).

If we assume all the dimensions are independent, then you are using **Naive Bayes Classifier**.
$$
P(\boldsymbol{x} \mid C_1) =
P(\begin{bmatrix}x_1 \\ x_2 \\ \vdots \\ x_K \end{bmatrix} \mid C_1) =
P(x_1 \mid C_1)P(x_2 \mid C_1) \dots P(x_K \mid C_1)
$$
Each $P(x_m \mid C_1)$ is now a 1-D Gaussian. For binary features, you may assume they are from Bernouli distributions. 

But if the assumption does not hold, the Naive Bayes Classifier may have a very high bias.

#### Posterior Probability

$$
\begin{align}
P(C_1 | x) 
&= \frac{P(x | C_1) P(C_1)}{P(x | C_1) P(C_1) + P(x | C_2) P(C_2)} \\ 
&= \frac{1}{1 + \frac{P(x | C_2) P(C_2)}{P(x | C_1) P(C_1)}} \\
&= \frac{1}{1 + \exp(-z)} \\ 
&= \sigma(z) \\
\end{align}
$$

$$
\begin{align}
z &= \ln \frac{P(x | C_1) P(C_1)}{P(x | C_2) P(C_2)} \\
&= \ln \frac{P(x | C_1)}{P(x | C_2)} + \ln \frac{P(C_1)}{P(C_2)} \\
&= \ln \frac{P(x | C_1)}{P(x | C_2)} + \ln \frac{\frac{N_1}{N_1+N_2}}{\frac{N_2}{N_1+N_2}} \\
&= \ln \frac{P(x | C_1)}{P(x | C_2)} + \ln \frac{N_1}{N_2} \\
\end{align}
$$

Furthermore:
$$
\begin{align}
\ln \frac{P(x | C_1)}{P(x | C_2)}
&= \ln \frac{\frac{1}{(2\pi)^{D/2} |\Sigma_1|^{1/2}} \exp\left\{-\frac{1}{2} (x - \mu^1)^T \Sigma_1^{-1} (x - \mu^1)\right\}}  {\frac{1}{(2\pi)^{D/2} |\Sigma_2|^{1/2}} \exp\left\{-\frac{1}{2} (x - \mu^2)^T \Sigma_2^{-1} (x - \mu^2)\right\}} \\
&= \ln \frac{|\Sigma_2|^{1/2}}{|\Sigma_1|^{1/2}} \exp \left\{ -\frac{1}{2} [(x - \mu^1)^T \Sigma_1^{-1} (x - \mu^1)-\frac{1}{2} (x - \mu^2)^T \Sigma_2^{-1} (x - \mu^2)] \right\} \\
&= \ln \frac{|\Sigma_2|^{1/2}}{|\Sigma_1|^{1/2}} - \frac{1}{2} \left[(x - \mu^1)^T \Sigma_1^{-1} (x - \mu^1) - (x - \mu^2)^T \Sigma_2^{-1} (x - \mu^2)\right]
\end{align}
$$
Further simplification goes:

<img src="assets/image-20240505132500740.png" alt="image-20240505132500740" style="zoom:33%;" />

Since we assume the distributions share the covariance matrix, we can further simplify the formula:

<img src="assets/image-20240505133107451.png" alt="image-20240505133107451" style="zoom:33%;" />
$$
P(C_1 \mid x) = \sigma(w^Tx + b)
$$
This is why the decision boundary is a linear line.

In generative models, we estimate $N_1, N_2, \mu^1, \mu^2, \Sigma$, then we have $\boldsymbol{w}$ and $b$. How about directly find $\boldsymbol{w}$ and $b$​?

### Logistic Regression

We want to find $P_{w,b}(C_1 \mid x)$. If $P_{w,b}(C_1 \mid x) \geq 0.5$, output $C_1$. Otherwise, output $C_2$.
$$
P_{w,b}(C_1 \mid x) = \sigma(z) = \sigma(w \cdot x + b) 
= \sigma(\sum_i w_ix_i + b)
$$
The function set is therefore (including all different $w$ and $b$):
$$
f_{w,b}(x) = P_{w,b}(C_1 \mid x)
$$
Given the training data $\{(x^1, C_1),(x^2, C_1),(x^3, C_2),\dots, (x^N, C_1)\}$, assume the data is generated based on $f_{w,b}(x) = P_{w,b}(C_1 \mid x)$. Given a set of $w$ and $b$, the probability of generating the data is:
$$
L(w,b) = f_{w,b}(x^1)f_{w,b}(x^2)\left(1-f_{w,b}(x^3)\right)...f_{w,b}(x^N)
$$

$$
w^*,b^* = \arg \max_{w,b} L(w,b)
$$

We can write the formulation by introducing $\hat{y}^i$, where:
$$
\hat{y}^i = \begin{cases}
1 & x^i \text{ belongs to } C_1 \\
0 & x^i \text{ belongs to } C_2
\end{cases}
$$
<img src="assets/image-20240505153535990.png" alt="image-20240505153535990" style="zoom:33%;" />

<img src="assets/image-20240505153917703.png" alt="image-20240505153917703" style="zoom:33%;" />
$$
C(p,q) = - \sum_x p(x) \ln \left( q(x) \right)
$$
Therefore, minimizing $- \ln L(w,b)$ is actually minimizing the cross entropy between two distributions: the output of function $f_{w,b}$ and the target $\hat{y}^n$​​.
$$
L(f) = \sum_n C(f(x^n), \hat{y}^n)
$$

$$
C(f(x^n), \hat{y}^n) = -[\hat{y}^n \ln f(x^n) + (1-\hat{y}^n) \ln \left(1-f(x^n)\right)]
$$

<img src="assets/image-20240505155715704.png" alt="image-20240505155715704" style="zoom:33%;" />

<img src="assets/image-20240505155812019.png" alt="image-20240505155812019" style="zoom:33%;" />

<img src="assets/image-20240505160902451.png" alt="image-20240505160902451" style="zoom:33%;" />

Here, the larger the difference ($\hat{y}^n - f_{w,b}(x^n)$) is, the larger the update.

Therefore, the update step for **logistic regression** is:
$$
w_i \leftarrow w_i - \eta \sum_n - \left(\hat{y}^n - f_{w,b}(x^n)\right)x_i^n
$$
This looks the same as the update step for linear regression. However, in logistic regression, $f_{w,b}, \hat{y}^n \in \{0,1\}$.

Comparision of the two algorithms:

<img src="assets/image-20240505161330795.png" alt="image-20240505161330795" style="zoom: 25%;" />

Why using square error instead of cross entropy on logistic regression is a bad idea?

<img src="assets/image-20240505163118191.png" alt="image-20240505163118191" style="zoom:25%;" />

<img src="assets/image-20240505163307888.png" alt="image-20240505163307888" style="zoom:25%;" />

In either case, this algorithm fails to produce effective optimization. A visualization of the loss functions for both cross entropy and square error is illustrated below:

<img src="assets/image-20240505163520499.png" alt="image-20240505163520499" style="zoom:25%;" />

### Discriminative v.s. Generative

The logistic regression is an example of **discriminative** model, while the Gaussian posterior probability method is an example of **generative** model, through their function set is the same.

<img src="assets/image-20240505170417654.png" alt="image-20240505170417654" style="zoom:25%;" />

We will not obtain the same set of $w$ and $b$. The same model (function set) but different function is selected by the same training data. The discriminative model tends to have a better performance than the generative model.

A toy example shows why the generative model tends to perform less well. We assume Naive Bayes here, namely $P(x \mid C_i) = P(x_1 \mid C_i)P(x_2 \mid C_i)$ if $x \in \mathbb{R}^2$. The result is counterintuitive -- we expect the testing data to be classified as Class 1 instead of Class 2.

<img src="assets/image-20240505202709608.png" alt="image-20240505202709608" style="zoom:25%;" />

<img src="assets/image-20240505211619095.png" alt="image-20240505211619095" style="zoom:25%;" />

### Multiclass Classification

<img src="assets/image-20240505213248614.png" alt="image-20240505213248614" style="zoom:33%;" />

**Softmax** will further enhance the maximum $z$ input, expanding the difference between a large value and a small value. Softmax is an approximation of the posterior probability. If we assume the previous Gaussian generative model that share the same covariance matrix amongst distributions, we can derive the exact same Softmax formulation. We can also derive Softmax from maximum entropy (similar to logistic regression).

<img src="assets/image-20240505213741874.png" alt="image-20240505213741874" style="zoom: 25%;" />

Like the binary classification case earlier, the multiclass classification aims to maximize likelihood, which is the same as minimizing cross entropy.

### Limitations of Logistic Regression

<img src="assets/image-20240505220032474.png" alt="image-20240505220032474" style="zoom: 25%;" />

Solution: **feature transformation**

<img src="assets/image-20240505220341723.png" alt="image-20240505220341723" style="zoom:25%;" />

However, it is *not* always easy to find a good transformation. We can **cascade logistic regression models**.

<img src="assets/image-20240505220557595.png" alt="image-20240505220557595" style="zoom: 25%;" />

<img src="assets/image-20240505220908418.png" alt="image-20240505220908418" style="zoom:25%;" />

# 3/04 Lecture 3: Image as input

## Preparation 1: 卷積神經網路 (Convolutional Neural Networks, CNN)

