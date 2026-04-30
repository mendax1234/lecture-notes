# Convolution Neural Network

## Convolution

Convolution is inherently a maths topic, however, it can be used with the neural network to enpower us to do something interesting on image processing. In this section, we will talk about the basics of convolution and how this idea of convolution can be used in image processing.

### Convolution Basics

The motivation of convolution starts from a single problem

> How do we combine two lists to get a third list, which is also equivalent to how do we combine two functions to get a third function.

Intuitively, we can add/multiply each element in the two lists and use the result as the elements in the third list. For example,

{% code lineNumbers="true" %}
```c
// Two list a and b, we want to get the third list c
a = [1, 2, 3]
b = [4, 5, 6]

// Addition
c = [5, 7, 9]

// Multiplication
c = [4, 10, 18]
```
{% endcode %}

Now, we want to introduce another way which is called **convolution** to combine these two lists/functions to get the third list/function. Intuitively, what convolution does is to **flip** the second list and uses it as a sliding window.

<figure><img src="../.gitbook/assets/convolution-sliding-windows-list.png" alt=""><figcaption></figcaption></figure>

Now, we can give the simpler definition of a convolution. The convolution of a list $$a_i$$ and another list $$b_i$$, denoted with the symbol $$*$$, is a **new list** and its $$n$$-th element is a sum shown below.

$$
(a*b)_n=\sum_{i,j,~i+j=n}a_i\cdot b_j
$$

<details>

<summary>Convolution and average sum</summary>

If the sum of the values in our sliding window (List B) is equal to 1, we can treat the value after convolution as an **average sum** of the values in List A.

<figure><img src="../.gitbook/assets/average-sum-example.png" alt=""><figcaption></figcaption></figure>

The figure above is not a strictly average some but assigned a higher weight to the data in the central.

</details>

### Image Processing

One worth noting example of using the idea of convolution is to do the image processing like:

1. Blur the image
2. Filter out the shape of the image

This is done by using a special sliding window called **kernel**. In this example, we assume that the kernel is a $$3\times3$$ matrix with each element to be $$\frac{1}{9}$$.

<figure><img src="../.gitbook/assets/blur-image-processing.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
**Pixel's RGB notation**

In each pixel of a normal image, we have three values, namly a number between 0 to 1 denoteing the pecentage of Red, Green and Blue of that pixel. That's why each element in the kernel is multiplied with a $$3\times1$$ vector.
{% endhint %}

Technically, when do the computation, we should flip the kernal matrix by 180 degree because of the pure math definition of the convolution. This aligns with the intuition that we flip the List B before using it as a sliding window. So, for a $$3\times3$$ matrix,

$$
A = \begin{pmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{pmatrix}
$$

Its 180 degree rotated version is

$$
A_{180} = \begin{pmatrix}
i & h & g \\
f & e & d \\
c & b & a
\end{pmatrix}
$$

But in our example, we assume that we are using the rotated kernel.

{% hint style="success" %}
It's the training/learning of CNN's job to get this kernel.
{% endhint %}

As we put the **centre element** of the kernel at the first pixel and start moving/sliding the kernel through all pixels of the image, the weighted sum will the be RGB value of the pixel at that centre position in the **convoluted image**.

{% hint style="warning" %}
It is definitely to change the size of the kernel matrix and the value inside it. By doing so will give us different image processing, like blurring or filtering out the shape of the figure, etc.
{% endhint %}

The purpose of the kernel can be understood as **finding the features/pattern** in the image. Usually we use one kernel for one pattern.

<details>

<summary>The size of the convolution</summary>

Smart readers might have found out that the size of the convolution (third list/image) might have a size bigger than the size of our input lists/images. Yes, this is valid! If our list A has a dimention of $$m$$ and our sliding window (list B) has a dimension of $$n$$, the size of the convolution will be $$n+m-1$$.

To deal with this problem, what we usually do is to trauncate the not used values. This can be shown as below.

<figure><img src="../.gitbook/assets/trancuate-convolution.png" alt=""><figcaption></figcaption></figure>

</details>

## CNN

A convolutional neural network (CNN) is a type of feedforward neural network that learns **features** via filter (or kernel) optimization. Here, we will use the image classification as an example. Assume that our input is an image, to recognize or classify that image, the flow/layers of CNN can be:

1. Convolution
2. Pooling
3. Activation

### Convolution

In the convolution, the process is similar as what we have mentioned in [#image-processing](convolution-neural-network.md#image-processing "mention"). The only difference is that to recognize an image accurately, we might use severl kernels, each kernel will be responsible for filtering our one feature from the input image, which is nothing but a matrix.

<figure><img src="../.gitbook/assets/kernel-filter-features.png" alt=""><figcaption></figcaption></figure>

The result after convolution using each kernel is also a very big matrix or maybe we might filter out some **weak** features/patterns. Thus, we need to compress the matrix.

{% hint style="warning" %}
**CNN vs. Plain Vanilla Neural Network**

In the CNN, the kernel operations replace the weights operation in the plain vanilla neural network.
{% endhint %}

### Pooling

The procedure of compressing the convoluted image is called **pooling**. Pooling can be as easy as getting the **mean** or **max** value of a **region**.

<figure><img src="../.gitbook/assets/pooling-explanation.png" alt=""><figcaption></figcaption></figure>

After the pooling, the size of the matrix/image will be lowered to a large extent.

<details>

<summary>Why is pooling valid?</summary>

In the input image, the neighboring pixels tend to have **similar** values. Thus, after convolution, the neighboring pixels in the convoluted image/matrix also tend to have **similar** values. This will cause the **information redundancy**. And pooling is used to lower this **information redundancy**.

</details>

### Activation

After the pooling, the **non-linear mapping** is applied to each element of the image/matrix after the pooling layer. This **non-linear mapping** is called the **activation function**. The intuition for doing so is to **reinforce** the features after the pooling layer.

### Training

We can expand the 2-dimension images into one-dimension and input them into a fully-connected neural network, or [Multi-Layer Perceptron](convolution-neural-network.md#multilayer-perceptron) (MLP) to get the value for the **kernel matrix** used in the convolution.

<figure><img src="../.gitbook/assets/from-cnn2mlp.png" alt=""><figcaption></figcaption></figure>

## Reference

1. [3Blue1Brown — What is a convolution](https://youtu.be/KuXjwB4LzSA?si=QBq3t7uadoE2KqXe).
