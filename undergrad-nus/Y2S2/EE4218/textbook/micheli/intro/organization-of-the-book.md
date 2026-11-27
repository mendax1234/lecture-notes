# Organization of the Book

This book presents techniques for **synthesis** and **optimization** of **combinational** and **sequential synchronous digital circuits**, starting from models at the **architectural level** and **logic level**.

## Abstract Model

Figure 1.20 represents schematically the **synthesis** and **optimization** tasks and their relations to the **models**.

<figure><img src="../../.gitbook/assets/circuit-models-synthesis-optimization (1).png" alt=""><figcaption><p>Figure 1.20 Circuit models, synthesis and optimization: a simplified view</p></figcaption></figure>

We assume that **circuit specification** is done by means of **HDL models**. **Synthesis** and **optimization algorithms** are described as being based on **abstract models** that are powerful enough to capture the **essential information** of the HDL models, and that **decouple synthesis from language-specific features**.

{% hint style="warning" %}
The sentence above is very awesome!
{% endhint %}

Examples of **abstract models** that can be derived from HDL models by **compilation** are:

1. **sequencing and data-flow graphs** (representing **operations** and **dependencies**),
2. **state transition diagrams** (describing **finite-state machine behavior**), and
3. **logic networks** (representing **interconnected logic blocks**, corresponding to **gates** or **logic functions**).

{% hint style="danger" %}
The rest content about what each chapter is about is on the book, very worth reading!
{% endhint %}

> TODO: Do a big picture here! From modeling to synthesis & optimization.
