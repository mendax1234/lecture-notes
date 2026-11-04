# Neural Network Training and Inferencing

In the previous section, we briefly introduced what **learning** means in a neural network:

> Learning is essentially the process of finding the optimal values for the network's **weights and biases**.

Now, we will explore two fundamental processes in neural networks: **training** and **inference**.

* **Training:** It's nothing but the learning process we've just briefly talked about, where the network adjusts its weights and biases to minimize the prediction error.
* **Inference:** The process of using the trained weights and biases to compute the output from a given input, layer by layer. This is also known as [**forward propagation**](https://wenbo-notes.gitbook.io/ee4218-notes/lab/neural-network/multilayer-perceptron#forward-propagation) (which we've also briefly talked about in the previous section!).

## Training

> This part is not very complete yet, for more intuitive understanding, please go to look at the 3B1B videos!

In short, the **training of a neural network** involves the following steps:

1. **Random Initialization:** Initialize the weights to small random values and the biases typically to zero.
2. **Forward Propagation:** Use the current weights and biases to compute the network's output, then evaluate the prediction error using a **loss function** (e.g., MSE or MAE).
3. **Backpropagation:** Calculate the gradients of the loss function with respect to each weight and bias, propagating backward from the output layer to the earlier layers.
4. **Gradient Descent:** Update the weights and biases using the calculated gradients to **minimize the loss function**.

Steps 2-4 are repeated iteratively until the network converges or a stopping criterion is met.

{% hint style="success" %}
**Training** is computationally expensive, but it is typically performed only once (or occasionally when the model needs to be updated). Nowadays, training is commonly accelerated using **GPUs**.
{% endhint %}

## Inference

On the other hand, **inference** is simply the **forward propagation** process discussed above or in the [previous section](https://wenbo-notes.gitbook.io/ee4218-notes/lab/neural-network/multilayer-perceptron#forward-propagation). Given an input and the trained weights and biases, the network computes its output layer by layer, without updating any parameters.

{% hint style="success" %}
Unlike training, **inference is performed repeatedly** whenever the model is used. Therefore, efficient inference is particularly important and is the main focus of our **EE4218 labs**. Nowadays, many companies develop specialized hardware, such as **ASIC-based AI accelerators** (e.g., Google's TPU), to accelerate neural network inference and improve energy efficiency.
{% endhint %}

## References

The references below might be useful to understand what's going on with the training of neural networks:

1. [3Blue1Brown — Gradient Descent, how neural networks learn](https://youtu.be/IHZwWFHWa-w?si=-G9HnXkdUp5nwBRC)
2. [3Blue1Brown — What is Backpropagation really doing](https://youtu.be/Ilg3gGewQ5U?si=HBjOe4BPk-tV_OvQ)
