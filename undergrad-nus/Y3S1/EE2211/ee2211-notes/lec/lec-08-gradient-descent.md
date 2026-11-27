# Lec 08 - Gradient Descent

Gradient descent is one of the **fundamental optimization** algorithms in modern machine learning. Before exploring how it works, we will first introduce three essential concepts: learning models, loss functions, and cost functions. Understanding these concepts will help us appreciate the motivation and intuition behind gradient descent.

## Important Concepts

### Learning Model

A learning model is essentially a function $$f$$ that maps an input $$x_i$$ to a predicted output $$\hat{y}_i$$, based on a set of learnable parameters $$\mathbf{w}$$:

$$
\hat{y}_i=f(x_i,w)
$$

Here, $$x_i$$ is the input, $$\mathbf{w}$$ represents the model parameters (e.g., weights), and $$\hat{y}_i$$ is the predicted output. For example, in linear regression, the learning model is $$\hat{\mathbf{y}}=\mathbf{X}\mathbf{w}$$, while in polynomial regression, it is $$\hat{\mathbf{y}}=\mathbf{P}\mathbf{w}$$, where $$\mathbf{X}$$ and $$\mathbf{P}$$ are the corresponding design matrices constructed from the training examples (each row represents one example, and each column represents one feature), and $$\mathbf{w}$$ contains the learnable parameters.

<details>

<summary>Other types of learning models</summary>

Beyond linear and polynomial regression, we will also explore other learning models of the form $$f(\mathbf{x}_i,\mathbf{w})=\sigma(\mathbf{p}_i^T\mathbf{w})$$, where $$\sigma$$ can be a nonlinear function such as ReLU, sigmoid, or exponential. In particular, ReLU and sigmoid are commonly used as activation functions in neural networks.

</details>

### Loss Function

A loss function measures the **error** between the predicted output $$\hat{y}_i$$ and the actual output $$y_i$$ for a single training example. Two commonly used loss functions in regression are:

1. Absolute Error (AE): $$L_i=|y_i-\hat{y}_i|$$
2. Squared Error (SE): $$L_i=(y_i-\hat{y}_i)^2$$

In general, we denote the loss for the $$i$$-th training example as:

$$
L_i=L(f(x_i,\mathbf{w}),y_i)
$$

### Cost Function

A cost function measures the **overall error** of a learning model across the entire training dataset by aggregating the individual losses. For $$n$$ training examples, two common cost functions based on squared error are:

1. Sum of Squared Errors (SSE):

$$
C(\mathbf{w})=\sum_{i=1}^{n}(y_i-\hat{y}_i)^2= \sum_{i=1}^m L(f(\mathbf{x}_i, \mathbf{w}), y_i)
$$

2. Mean Squared Error (MSE):

$$
C(\mathbf{w}) = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2=\frac{1}{n} \sum_{i=1}^m L(f(\mathbf{x}_i, \mathbf{w}), y_i)
$$

### Regularization

Previously, we introduced the **cost function**, which measures the overall fitting error of a learning model across the training dataset. However, minimizing only the training error may lead to **overfitting**, where the model performs very well on the training data but poorly on unseen data.

To address this, we introduce **regularization**, which penalizes model complexity and encourages the model to generalize better to unseen data. The general cost function with regularization becomes:

$$
C(\mathbf{w})=\underbrace{\sum_{i=1}^{m}L(f(\mathbf{x}_i,\mathbf{w}),y_i)}_{\text{Data Loss}}+\underbrace{\lambda R(\mathbf{w})}_{\text{Regularization}}
$$

Here:

* **Data Loss:** Measures how well the model fits the training data. A smaller data loss indicates a better fit.
* **Regularization (**$$R(\mathbf{w})$$**):** Penalizes model complexity, typically by penalizing large weights.
* **Regularization hyperparameter (**$$\lambda\geq0$$**):** Controls the strength of regularization.

The regularization hyperparameter $$\lambda$$ determines the trade-off between **fitting the training data** and **keeping the model simple**:

* **Increasing** $$\lambda$$**:** Stronger regularization, encouraging smaller weights and a less complex model. This generally reduces overfitting but may increase training error.
* **Decreasing** $$\lambda$$**:** Weaker regularization, allowing the model to fit the training data more closely. This generally reduces training error but may increase overfitting.
* **When** $$\lambda=0$$**:** No regularization is applied, so the cost function consists only of the data loss.

For example, in [**ridge regression**](lec-06-polynomial-ridge-regression-and-classification.md#ridge-regression), we use **L2 regularization**, where:

$$
R(\mathbf{w})=\mathbf{w}^T\mathbf{w}=\sum_{j=1}^{n}w_j^2
$$

Thus, the cost function for polynomial regression with ridge regularization is:

$$
C(\mathbf{w})=(\mathbf{P}\mathbf{w}-\mathbf{y})^T(\mathbf{P}\mathbf{w}-\mathbf{y})+\lambda\mathbf{w}^T\mathbf{w}
$$

#### Relationship Between Regularization and Overfitting

Recall that **overfitting** occurs when a model performs well on the training set but poorly on unseen data, whereas **underfitting** occurs when a model is too simple to capture the underlying relationship.

* **Overfitting:** We can potentially improve generalization by increasing regularization, decreasing model complexity, or removing irrelevant features.
* **Underfitting:** We can potentially improve the model by decreasing regularization, increasing model complexity, or adding useful features.

However, stronger regularization does not always improve test accuracy. Excessive regularization may cause underfitting, so the appropriate value of $$\lambda$$ should be selected using validation data.

#### Effect of Increasing the Regularization Hyperparameter

Consider the general cost function:

$$
C_\lambda(\mathbf{w})=D(\mathbf{w})+\lambda R(\mathbf{w})
$$

where $$D(\mathbf{w})$$ represents the data loss and $$R(\mathbf{w})$$ represents the regularization penalty. Suppose we **increase** $$\lambda$$ and find the new global minimum of the cost function. In general:

1. **The optimal regularization penalty decreases or stays the same**, because the optimization places more emphasis on minimizing $$R(\mathbf{w})$$.
2. **The optimal data loss increases or stays the same**, because the optimization allows a worse fit to the training data in exchange for a smaller regularization penalty.

Importantly, **the total optimal cost does not necessarily decrease** when $$\lambda$$ increases. In fact, assuming $$R(\mathbf{w})\geq0$$, the minimum total cost can only increase or stay the same, because the regularization term receives a larger nonnegative weight.

## Gradient Descent

#### Motivation for Gradient Descent

In the previous lectures, we introduced two machine learning methods: [linear regression](lec-05-linear-regression.md) and [polynomial regression](lec-06-polynomial-ridge-regression-and-classification.md). Both aim to find the optimal model parameters $$\mathbf{w}$$ (including weights and bias) that minimize the cost function. Mathematically, this can be expressed as:

$$
\underset{\mathbf{w}}{\operatorname{argmin}}\ C(\mathbf{w}) = \underset{\mathbf{w}}{\operatorname{argmin}} \sum_{i=1}^{m}L(f(\mathbf{x}_i,\mathbf{w}),y_i)
$$

For linear and polynomial regression, we can find the optimal parameters analytically by solving linear equations, using the inverse or pseudoinverse (primal/dual formulations).

{% hint style="warning" %}
Gradient descent can also be used in linear and polynomial regression but it is not recommended as we already can find the optimal solution analytically.
{% endhint %}

However, what if we want to fit our data using a different model, such as an exponential function? Unlike linear and polynomial regression, such models generally do not have a straightforward closed-form solution for their optimal parameters.

This motivates gradient descent, an iterative optimization algorithm that updates the model parameters step by step to minimize the cost function, without requiring a closed-form solution.

{% hint style="success" %}
The key idea is that gradient descent provides a general approach to optimizing differentiable models, including those for which analytical solutions are difficult or unavailable.
{% endhint %}

The idea behind gradient descent is simple:

> instead of solving directly for the optimal parameters, we **iteratively** update the parameters in the direction that decreases the cost function most rapidly.

Suppose we want to minimize the cost function $$C(\mathbf{w})$$, where the model parameters are:

$$
\mathbf{w}= \begin{bmatrix} w_1 & w_2 & \cdots & w_d \end{bmatrix}^{T}
$$

The gradient of the cost function is a vector containing its partial derivatives with respect to each model parameter:

$$
\nabla_{\mathbf{w}}C(\mathbf{w})= \begin{bmatrix} \frac{\partial C}{\partial w_1}\\[6pt] \frac{\partial C}{\partial w_2}\\ \vdots\\ \frac{\partial C}{\partial w_d} \end{bmatrix}
$$

The gradient has two important properties:

* $$\nabla_{\mathbf{w}}C(\mathbf{w})$$ points in the direction of the **steepest increase** in the cost function.
* $$-\nabla_{\mathbf{w}}C(\mathbf{w})$$ points in the direction of the **steepest decrease** in the cost function.

Therefore, to minimize the cost function, we should update the parameters in the **negative gradient** direction. Now, here it comes the most important part, the gradient descent update rules! Gradient descent repeatedly updates the parameters using:

$$
\mathbf{w}_{k+1}=\mathbf{w}_k-\eta\nabla_{\mathbf{w}}C(\mathbf{w}_k)
$$

where:

* $$\mathbf{w}_k$$: Current model parameters at iteration $$k$$.
* $$\eta$$: Learning rate, which controls the step size.
* $$\nabla_{\mathbf{w}}C(\mathbf{w}_k)$$: Gradient evaluated at the current parameters.

The algorithm follows three steps:

1. Initialize the parameters $$\mathbf{w}_0$$ and choose a learning rate $$\eta$$.
2. Update the parameters using the gradient descent rule.
3. Repeat until a **convergence** criterion is satisfied.

For a sufficiently small learning rate and a nonzero gradient, each update generally decreases the cost function:

$$
C(\mathbf{w}_{k+1})<C(\mathbf{w}_k)
$$

The learning rate $$\eta$$ determines **how far we move along the negative gradient direction** in each iteration. Choosing an appropriate learning rate is important for convergence.

<details>

<summary>Effect of the learning rate</summary>

**Small learning rate**: Takes small steps, resulting in slow but potentially stable convergence. More iterations are needed to reach the minimum.

{% hint style="warning" %}
An important consideration is that a smaller learning rate does not necessarily lead to a **better minimum**. If the cost function has multiple local minima, gradient descent may converge to a local minimum depending on its initialization and optimization path.

However, if the cost function is convex and has a unique global minimum, a sufficiently small learning rate can allow gradient descent to reach that minimum given enough iterations and suitable convergence conditions.
{% endhint %}

</details>

Gradient descent continues until a stopping convergence criterion is satisfied. Common criteria include:

1. **Maximum iterations**: Stop after a predefined number of iterations.
2. **Change in cost**: Stop when the absolute or relative change in $$C(\mathbf{w})$$ falls below a threshold.
3. **Change in parameters**: Stop when the absolute or relative change in $$\mathbf{w}$$ falls below a threshold.

These criteria help determine when further updates are unlikely to provide meaningful improvement.

<details>

<summary>Local vs. Global Minimum</summary>

A major limitation of gradient descent is that it is not guaranteed to find the global minimum for a non-convex cost function.

* **Local minimum**: A point where the cost is lower than at nearby points, but not necessarily the lowest overall.
* **Global minimum**: The point with the lowest cost across the entire parameter space.

<figure><img src="../.gitbook/assets/local-global-minimum (1).png" alt="" width="563"><figcaption></figcaption></figure>

At a differentiable local minimum, the gradient is zero:

<p align="center"><span class="math">\nabla_{\mathbf{w}}C(\mathbf{w})=\mathbf{0}</span></p>

Consequently, gradient descent **cannot move away** from that point using the standard update rule, even if a better minimum exists elsewhere.

{% hint style="warning" %}
Gradient descent can also converge to a global minimum, particularly for convex cost functions. Its outcome depends on the shape of the cost function, initialization, and learning rate.
{% endhint %}

</details>
