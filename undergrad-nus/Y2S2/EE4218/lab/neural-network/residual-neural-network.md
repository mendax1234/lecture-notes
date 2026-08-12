# Residual Neural Network

## Introduction

{% hint style="warning" %}
This part is mainly from He's famous paper on Deep Residual Learning for Image Recognition, feel free to go back and read the paper again at any time! It's very worth reading!
{% endhint %}

In this section, we will introduce the backbone of ResNet — _Deep Residual Learning_ framework by starting from a problem

> How can we learn better networks?

Remember that in an [MLP](multilayer-perceptron.md#why-order-neurons-into-layers), different layers can learn features at different levels of abstraction. Earlier layers tend to learn simpler patterns, while deeper layers combine these patterns into more complex, higher-level features that are more useful for producing the final output.

By stacking multiple layers, a deep neural network can learn a hierarchy of features — often described as low-, mid-, and high-level representations — in an end-to-end multilayer manner. Ideally, the "levels" of features can be enriched by the number of stacked layers (depth).

However, more and more papers and experiments show that this is **not true**! The authors of _"Deep Residual Learning for Image Recognition"_ thus come up with a very thought-provoking question:

> Is learning better networks as easy as stacking more layers?

This is obviously **not true** because of the problem of [_vanishing/exploding gradients_](https://en.wikipedia.org/wiki/Vanishing_gradient_problem)_,_ which can be understood as the problem of greatly diverging [gradient](https://en.wikipedia.org/wiki/Gradient) magnitudes between **earlier** and **later layers** encountered when training neural networks with backpropagation.

However, this vanishing/exploding gradient problem can be mitigated by several techniques, including:

1. **Proper Weight Initialization:** Initialize **weights** at an appropriate scale so that activations and gradients do not become extremely large or small as they pass through many layers.
2. **Normalization Layers:** Layers such as [_Batch Normalization_](https://en.wikipedia.org/wiki/Batch_normalization) normalize intermediate activations during training by adjusting the inputs to each layer — re-centering them around zero and re-scaling them to a standard size. This can make training more stable and often allows deep networks to converge faster.

When deeper networks are able to start converging, a _degradation_ problem has been exposed: with the network depth increasing, accuracy gets saturated (which might be unsurprising) and then degrades rapidly. Unexpectedly, such degradation is not caused by _overfitting_, which can be briefly understood as having low training error but high test error, and adding more layers to a suitably deep model leads to **higher training errors**. The following figure shows a typical example.

<figure><img src="../.gitbook/assets/degradation.png" alt=""><figcaption><p>Figure 1: Training error (left) and test error (right) on CIFAR-10 with 20-layer and 56-layer "plain" networks.</p></figcaption></figure>

The degradation (of training accuracy) indicates that not all systems are similarly easy to optimize. Let us consider a shallower architecture and its deeper counterpart that adds more layers onto it. There exists a solution by construction to the deeper model: the added layers are _identity mapping_, and the other layers are **copied** from the learned shallower model. The existence of this constructed solution indicates that a deeper model should produce no higher training error than its shallower counterpart. But experiments show that our current solvers on hand like stochastic gradient descent (SGD) are unable to find solutions that are comparably good or better than the constructed solution (or unable to do so in feasible time), meaning that the solutions found by the current solvers for the newly added layers are **neither** identity maps **nor** mappings that are better than identity maps.

In He's paper, the authors address this degradation problem by introducing a _deep residual learning_ framework. Instead of hoping each few stacked layers directly fit a desired underlying mapping, we explicitly let these layers fit a **residual mapping**. Formally, denoting the desired underlying mapping as $$H(x)$$, we let the stacked nonlinear layers fit another mapping of $$F(x):=H(x)-x$$ (This is called the **residual mapping** and it usually can be understood as the difference between the desired output and the actual input). The original mapping is recast into $$F(x)+x$$. We hypothesize that it is easier to optimize the residual mapping than to optimize the original, unreferenced mapping.

{% hint style="success" %}
To the extreme, if an identity mapping were optimal, it would be easier to push the **residual to zero** than to fit an identity mapping by a stack of nonlinear layers.
{% endhint %}

The formulation of $$F(x)+x$$ can be realized by feedforward neural networks with "shortcut connections" (Fig. 2). **Shortcut connections**&#x20;are those skipping one or more layers. In our case, the shortcut connections simply perform identity mapping, and their outputs are added to the outputs of the stacked layers (Fig. 2).

<figure><img src="../.gitbook/assets/residual-learning-building-block.png" alt=""><figcaption><p>Figure 2: Residual learning: a building block</p></figcaption></figure>

Identity shortcut connections add neither extra parameter nor computational complexity. The entire network can still be trained end-to-end by SGD with backpropagation, and can be easily implemented using common libraries without modifying the solvers.

## Deep Residual Learning

### Residual Learning

Let us consider $$H(x)$$ as an underlying mapping to be fit by a few stacked layers (not necessarily the entire net), with $$x$$ denoting the inputs to the first of these layers. If one hypothesizes that multiple nonlinear layers can asymptotically approximate complicated functions, then it is equivalent to hypothesize that they can asymptotically approximate the residual functions, i.e., $$H(x)−x$$(assuming that the input and output are of the same dimensions). So rather than expect stacked layers to approximate $$H(x)$$, we explicitly let these layers approximate a residual function $$F(x):=H(x)−x$$. The original function thus becomes $$F(x)+x$$. Although both forms should be able to asymptotically approximate the desired functions (as hypothesized), the ease of learning might be different.

This reformulation is motivated by the counterintuitive phenomena about the degradation problem (Fig. 1). As we discussed in the introduction, if the added layers can be constructed as identity mappings, a deeper model should have training error no greater than its shallower counterpart. The degradation problem suggests that the solvers might have difficulties in approximating identity mappings by multiple nonlinear layers. With the residual learning reformulation, if identity mappings are optimal, the solvers may simply drive the weights of the multiple nonlinear layers toward zero to approach identity mappings.

{% hint style="warning" %}
The last sentence holds because we are talking about the **residual block** here but not the traditional neural network. If it's the traditional neural network, for a layer to become an identity mapping, the weights of a layer should be an identity matrix and the bias should be 0.
{% endhint %}

In real cases, it is unlikely that identity mappings are optimal, but our reformulation may help to precondition the problem. If the optimal function is closer to an identity mapping than to a zero mapping, it should be easier for the solver to find the perturbations with reference to an identity mapping, than to learn the function as a new one.

{% hint style="success" %}
**Pertubations** can be understood as the minor adjustments or tweaks needed to improve the feature representation and it is significantly easier for a solver like Stochastic Gradient Descent to learn a small change than to reconstruct the entire underlying function.
{% endhint %}

### Identity Mapping by Shortcuts

We adopt residual learning to every few stacked layers. A building block is shown in Fig. 2. Formally, in this paper we consider a building block defined as:

$$
y=\mathcal{F}(x,\{W_i\})+x \tag1
$$

Here $$x$$ and $$y$$ are are the input and output vectors of the layers considered. The function $$\mathcal{F}(x,\{W_i\})$$ represents the **residual mapping** to be learned. For the example in Fig. 2 that has two layers, $$\mathcal{F}=W_2\sigma(W_1x)$$ in which $$\sigma$$ denotes ReLU and the biases are omitted for simplifying notations. The operation $$\mathcal{F}+x$$ is performed by a shortcut connection and element-wise addition.

{% hint style="success" %}
Please go back to our previous notes on [matrix notation](multilayer-perceptron.md#matrix-notation) in the plain vanilla, as the example here is nothing but two fully-connected layers.
{% endhint %}

The shortcut connections in Eqn. (a) introduce neither extra parameter nor computation complexity.

The dimensions of $$x$$ and $$\mathcal{F}$$ must be equal in Eqn. (1). If this is not the case (e.g., when changing the input/output channels), we can perform a linear projection $$W_s$$ by the shortcut connections to match the dimensions:<br>

$$
y = \mathcal{F}(x, \{W_i\}) + W_s x \tag{2}
$$

{% hint style="info" %}
For example, if our input vector is $$128\times1$$ and our output vector is $$64\times1$$, then the [**learnable**](#user-content-fn-1)[^1] linear projection matrix will have the dimension of $$64\times128$$.
{% endhint %}

We can also use a square matrix $$W_s$$ in Eqn. (1). But we will show by experiments that the identity mapping is sufficient for addressing the degradation problem and is economical, and thus $$W_s$$ is only used when matching dimensions.

The form of the residual function $$\mathcal{F}$$ is flexible. Experiments in this paper involve a function $$\mathcal{F}$$ that has two or three layers (Fig. 5), while more layers are possible. But if $$\mathcal{F}$$ has only a single layer, Eqn. (1) is similar to a linear layer: $$y = W_1 x + x$$, for which we have not observed advantages.

We also note that although the above notations are about fully-connected layers for simplicity, they are applicable to convolutional layers. The function $$\mathcal{F}(x, \{W_i\})$$ can represent multiple convolutional layers. The element-wise addition is performed on two feature maps, channel by channel.

### Network Architectures

We have tested various plain/residual nets, and have observed consistent phenomena. To provide instances for discussion, we describe two models for ImageNet as follows.

#### Plain Network

Our plain baselines (Fig. 3, middle) are mainly inspired by the philosophy of VGG nets (Fig. 3, left). The convolutional layers mostly have 3×3 filters and follow two simple design rules:

1. for the same output feature map size, the layers have the same number of filters; and
2. if the feature map size is halved, the number of filters is doubled so as to preserve the time complexity per layer.

We perform downsampling directly by convolutional layers that have a [stride](convolution-neural-network.md#stride) of 2. The network ends with a global average pooling layer and a 1000-way fully-connected layer with softmax. The total number of weighted layers is 34 in Fig. 3 (middle).

{% hint style="warning" %}
Compared with **VGG-19**, which uses **max-pooling layers** for downsampling, the **34-layer plain network** performs downsampling using **convolutional layers with a stride of 2**.
{% endhint %}

It is worth noticing that our model has fewer filters and lower complexity than VGG nets (Fig. 3, left). Our 34-layer baseline has 3.6 billion FLOPs (multiply-adds), which is only 18% of VGG-19 (19.6 billion FLOPs).

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="515"><figcaption><p>Figure 3. Example network architectures for ImageNet. Left: the VGG-19 model as a refernce. Mid: a plain network with 34 parameter layers (3.6 billion FLOPs). Right: a residual network with 34 parameter layers (3.6 billion FLOPs). The dotted shortcuts increase dimensions.</p></figcaption></figure>

{% hint style="success" %}
#### Label Notation Meaning

When we see a label like `3x3 conv, 512` inside one of the network blocks, it acts as a shorthand formula for the parameters of that specific layer. Here is exactly what each part means:

* `3x3`: This is the spatial size of the convolutional filter (or kernel). It means the filter sliding over the image is 3 pixels wide by 3 pixels high.
* `conv`: This indicates the type of mathematical operation being performed, which is a standard convolution.
* `512`: This represents the number of independent filters used in that layer. Because there are 512 filters, this layer will output a feature map with a depth of 512 channels.
* `/2` (e.g., `3x3 conv, 128, /2`): When we see a `/2` at the end, it means the layer uses a stride of 2.
{% endhint %}

#### Residual Network

Based on the above plain network, we insert shortcut connections (Fig. 3, right) which turn the network into its counterpart residual version. The identity shortcuts (Eqn. (1)) can be directly used when the input and output are of the **same dimensions** (solid line shortcuts in Fig. 3). When the dimensions increase (dotted line shortcuts in Fig. 3), we consider two options:

1. The shortcut still performs identity mapping, with extra zero entries padded for increasing dimensions. This option introduces no extra parameter;
   1. Example: To transition from a $$56 \times 56 \times 64$$ input to a $$28 \times 28 \times 128$$ output, the shortcut spatially skips every other pixel (stride of 2) and concatenates 64 channels of pure zeros to the depth. (Total learnable weights = 0).
2. The projection shortcut in Eqn. (2) is used to match dimensions (done by $$1\times1$$ convolutions).
   1. Example: To transition from a $$56 \times 56 \times 64$$ input to a $$28 \times 28 \times 128$$ output, the shortcut applies 128 independent $$1 \times 1$$ filters (each with a depth of 64) using a stride of 2. (Total learnable weights = $$1 \times 1 \times 64 \times 128 = 8,192$$).

For both options, when the shortcuts go across feature maps of two sizes, they are performed with a stride of 2.

## References

1. [Deep Residual Learning for Image Recognition by Kaiming He](https://arxiv.org/abs/1512.03385).
2. [The interpretation of He's paper by Mu Li](https://www.bilibili.com/video/BV1P3411y7nn/?share_source=copy_web\&vd_source=38953bcabbabbab600e123d8740d5a8a).

[^1]: By "learnable" I mean that this matrix can be treated as a weight and that's why its notation is $$W_s$$.
