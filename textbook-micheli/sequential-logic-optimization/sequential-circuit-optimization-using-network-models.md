# Sequential Circuit Optimization using Network Models

We already mentioned in the [**introduction**](introduction.md) **to this chapter** that the **behavior of sequential circuits** can be described by **traces**, i.e., by **sequences of inputs and outputs**.

* These sequences correspond to those that the related **finite-state machine models** can **accept and generate**.

We recall that we restrict our attention to **synchronous circuits** with **single-phase edge-triggered registers** for the sake of **simplicity**.

* For this reason, it is convenient to **discretize time** into an **infinite set of time points** corresponding to the **set of integers** and to the **triggering conditions of the registers**.

We assume that the observed **operation of the network** begins at time $$n = 0$$ after an appropriate **initializing (or reset) sequence** is applied.

* By this choice, the **reset inputs** are applied at some time point $$n \le 0$$.

#### Some Notations

Now are introduce two notations that will be used in representing the **network models** as logic expressions

{% stepper %}
{% step %}
#### Normal Notation

We denote sequences by **time-labeled variables**.

* For example, $$x(n)$$ denotes **variable x at time n**.
* A sequence of values of variable $$x$$ is denoted by $$x^{(n)}\forall n$$ in an **interval of interest**.
{% endstep %}

{% step %}
#### Shorthand Notation

It is convenient sometimes to have a **shorthand notation for variables**, without explicit dependency on time but just marking the **synchronous delay offset** with respect to a **reference time point**.

* We represent this by appending to the variable the **reserved symbol @** and the **offset** when this is different from zero.
* Hence, $$x@k = x^{(n-k)}$$ and $$x = x@0$$.

**Circuit equations** in the shorthand notation are **normalized** by assuming that the **left-hand side has zero offset**.

* For example, $$x^{(n+1)} = y^{(n)}$$ is translated to $$x=y@1$$.
{% endstep %}
{% endstepper %}

<details>

<summary>Example of using logic expression to represent network models</summary>

Consider the **circuit of Figure 9.2(b)**, which provides an **oscillating sequence** when input **r** is **FALSE**.

<figure><img src="../../.gitbook/assets/synchronous-logic-network.png" alt="" width="251"><figcaption><p>Figure 9.2(b)</p></figcaption></figure>

Its **behavior** can be expressed as $$z^{(n)} = (z^{(n-1)} + r^{(n)})'$$, for all $$n \ge 0$$. Input **r** is a **reset condition**. Using the **shorthand notation**, the circuit can be described by the expression $$z = (z@1 + r)'$$.

</details>

#### Unbound and Bound Networks

**Network models for synchronous circuits** are **extensions** of those used for describing **multiple-level combinational circuits**. In particular, **logic networks** where **modules can represent storage elements** can be used to represent both **bound** and **unbound networks**:

* **Bound networks** represent **gate-level implementation**.
  * It uses **library cells**.
* **Unbound networks** associate **internal modules** with **logic expressions** in terms of **time-labeled variables**, representing a **structured way** of describing **circuit behavior**.

{% hint style="warning" %}
We consider **unbound networks** in this section.
{% endhint %}

#### Definition of the synchronous logic network

As in the case of **combinational synthesis**, we restrict our attention to **non-hierarchical logic networks**, and we assume that the **interconnection nets** are split into sets of **two-terminal nets**. In addition, we represent the **registers implicitly**, by **positive weights** assigned to the **nets** (and to the **edges** in the corresponding **graph**), where the weights denote the corresponding **synchronous delays**:

* A **direct connection** between two **combinational modules** has **zero weight**.
* A **connection through a register** has **unit weight**.
* A **connection through a k-stage shift register** has weight **k**.
* **Zero weights** are sometimes omitted in the **graphical representations**.

We define a **path weight** as the **sum of the edge weights** along that path.

* _(Path weights should not be confused with **path delays**, which are the **composition of vertex propagation delays**.)_

When compared to **combinational logic networks**, **synchronous networks** differ in:

* Being **edge-weighted**.
* Not being restricted to **acyclic graphs**.

Nevertheless, the **synchronous circuit assumption** requires each **cycle** to have **positive weight** to **disallow direct combinational feedback**. We summarize these considerations by refining the earlier **definition of logic network**.

> **Definition 9.3.1.** A **non-hierarchical synchronous logic network** is:
>
> * A **set of vertices** $$V$$ partitioned into three subsets:
>   * **Primary inputs** $$V^I$$​
>   * **Primary outputs** $$V^O$$​
>   * **Internal vertices** $$V^G$$
> * Each **vertex** is assigned to a **variable**.
> * A **set of scalar combinational Boolean functions** associated with the **internal vertices**.
>   * The **support variables** of each **local function** are **time-labeled variables** associated with **primary inputs** or other **internal vertices**.
>   * The **dependency relation** of the support variables corresponds to the **edge set** $$E$$ and the **synchronous delay offsets**, i.e., differences in **time labels** correspond to the **edge weights** $$W$$.
> * A **set of assignments of the primary outputs** to **internal vertices**, denoting which **variables** are directly **observable from outside the network**.

A **synchronous logic network** can be represented using the following two ways

{% stepper %}
{% step %}
#### Multi-graph

Note that a **local function** may depend on the value of a **variable** at **different instances of time**.

* In this case, the model requires **multiple edges** between the corresponding **vertices** with the appropriate **weights**.
* Thus, a **synchronous network** is modeled by a **multi-graph**, denoted by $$G_{sn} = (V, E, W)$$.

Note that **synchronous logic networks** **simplify** to **combinational networks** when **no registers** are present.

<details>

<summary>Example of using multi-graph to represent a synchronous logic model</summary>

An example of a **synchronous circuit** and its **network model** are shown in **Figure 9.7**.

<figure><img src="../../.gitbook/assets/synchronous-logic-network-example.png" alt=""><figcaption><p>Figure 9.7 Synchronous circuit and synchronous logic network</p></figcaption></figure>

The **circuit** decodes an **incoming data stream** coded with **bi-phase marks**, as produced by a **compact-disk player**. The **network model** is a **multi-graph**. For example, there are **two edges** between $$u_1$$ and $$v_1$$​, with **zero** and **unit weight**. Note that **zero weights** are not shown. There are **two unit-weighted cycles**.

</details>
{% endstep %}

{% step %}
#### Logic Equations

As in the case of **combinational networks**, an **alternative representation** is possible in terms of **logic equations**, whose **support** consists of **variables with explicit dependence on time**.

<details>

<summary>Example of using logic expressions to represent a synchronous logic model</summary>

Consider the **network of Figure 9.7**. It can be described by the following **set of equations**:

<figure><img src="../../.gitbook/assets/logic-expression-example-1.png" alt="" width="279"><figcaption></figcaption></figure>

or equivalently, using the shorthand notation,

<figure><img src="../../.gitbook/assets/shorthand-notation-example.png" alt="" width="218"><figcaption></figcaption></figure>

</details>
{% endstep %}
{% endstepper %}

There are different approaches to optimizing synchronous networks

{% stepper %}
{% step %}
#### Combinational Optimization

The **simplest** approach is to **ignore the registers** and to **optimize the combinational component** using **techniques of combinational logic synthesis**. This is equivalent to **deleting the edges with positive weights** and **optimizing the corresponding combinational logic network**. Needless to say, the **removal of the registers** from the **network** **segments the circuit** and **weakens the optimality**.
{% endstep %}

{% step %}
#### Retiming

A **radically different approach** is **retiming**. By **retiming a network**, we **move the position of the registers only**; hence, we do **not change the graph topology**, but we **modify the weight set** $$W$$. **Leiserson and Saxe** presented **polynomially bounded algorithms** for finding the **optimum retiming**, which **minimizes the circuit cycle-time or area**. **Unfortunately, retiming** may **not lead to the best implementation**, because **only register movement** is considered.
{% endstep %}

{% step %}
#### Network Transformation

The **most general approach** to **synchronous logic optimization** is to perform **network transformations** that **blend retiming** with **combinational transformations**. Such transformations can have an **algebraic** or **Boolean flavor**. In the latter case, the concept of **don’t-care conditions** must be **extended to synchronous networks**.
{% endstep %}
{% endstepper %}

We present **retiming** first. Then we survey **recent results** on **synchronous logic transformations** as well as on **enhancements** to the original **retiming method**. We conclude this section by describing the **specification of don’t-care conditions** for **optimizing synchronous networks**.

{% hint style="warning" %}
The combination of **retiming** and **network transformation** is combined into one thing call "[RTL Transformation](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/lecture/lec-02/lec-02b-rtl-transformations)" which is introduced in NUS EE4415.
{% endhint %}
