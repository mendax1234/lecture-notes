# Sequential Logic Optimization

We consider in this chapter the optimization of sequential circuits modeled by finite-state machines at the logic level. We restrict our attention to synchronous models that operate on a [single-phase clock](#user-content-fn-1)[^1]. From a circuit standpoint, they consist of **interconnections** of combinational logic gates and registers. Thus, synchronous sequential circuits are often modeled by a combinational circuit component and registers, as shown in Figure 9.1.

<figure><img src="../../.gitbook/assets/block-diagram-fsm.png" alt="" width="563"><figcaption><p>Figure 9.1 Block diagram of a finite-state machine implementation.</p></figcaption></figure>

{% hint style="warning" %}
We assume here, for the sake of simplicity, that registers are edge-triggered and store 1 bit of information. **This assumption allows us to model each register as a binary state element.**
{% endhint %}

The sequential circuit model may have either a **behavioral** or a **structural** flavor, or a combination of both. Whereas the behavior of combinational logic circuits can be expressed by **logic functions**, the behavior of sequential circuits can be captured by _**traces**_, i.e., by input and output sequences. In this section, we will introduce two ways to represent the **synchronous circuit behavior.**

{% stepper %}
{% step %}
### State-Based Model

A convenient way to express the circuit behavior is by means of **finite-state machine models/state-based models.** One example will be the **state transition diagrams**, as shown in Figure 9.2 (a). **State transition diagrams** encapsulate the **traces** that the corresponding circuit can accept and produce. Thus, **state-based representations** have a **behavioral flavor**.

<figure><img src="../../.gitbook/assets/state-transition-diagram.png" alt="" width="462"><figcaption><p>Figure 9.2(a) State transition diagram</p></figcaption></figure>

{% hint style="warning" %}
In the diagram above, just treat r as 1 and r' as 0, this will make the state transition diagram easier to understand. This image above is actually composed of one state transition diagram and its corresponding state transition table.
{% endhint %}

While many optimization techniques have been proposed, this finite-state machine representation **lacks** a **direct relation** between

1. **state manipulation** (e.g., state minimization) and
2. the corresponding **area and delay variations**.

In general, those optimization techniques **yield** a reduction in **model complexity** that correlates well with **area reduction**, but not necessarily with **performance improvement**.

{% hint style="success" %}
The state-based models can be represented by

1. A state-transition diagram
2. A table which has the information about output and next-state.
{% endhint %}
{% endstep %}

{% step %}
### Structural model

An alternative representation of the circuit behavior is by means of **logic expressions** in terms of **time-labeled variables**. As in the case of **combinational circuits**, it is often convenient to express the **input/output behavior** by means of a set of **local expressions** with **mutual dependencies**. This leads to circuit representations in terms of **synchronous logic networks** that express the interconnection of **combinational modules** and **registers**, as shown in Figure 9.2 (b).

<figure><img src="../../.gitbook/assets/synchronous-logic-network.png" alt="" width="377"><figcaption><p>Figure 9.2(b) Synchronous Logic Network</p></figcaption></figure>

**Synchronous logic networks** have a **structural flavor** when the combinational modules correspond to **logic gates**. They are **hybrid structural/behavioral views** of the circuit when the modules are associated with **logic functions**. Some recent **optimization algorithms for sequential circuits** use the **network representation**, such as **retiming**. In this case, there is a **direct relation** between **circuit transformations** and **area and/or performance improvement**. However, the notion of **state** is **implicit** in the synchronous logic networks.

{% hint style="success" %}
A structural model can be represented by

1. A structural diagram consisting of different logic gates and FFs.
2. a logic expression in terms of time-labeled variables.
{% endhint %}
{% endstep %}
{% endstepper %}

**State transition diagrams** can be transformed into synchronous logic networks by **state encoding** and can be recovered from synchronous logic networks by **state extraction**.

{% stepper %}
{% step %}
#### State Encoding

**State encoding** defines the **state representation** in terms of **state variables**, thus allowing the description in terms of **networks**. **Unused state codes** are **don’t care conditions** that can be used for **network optimization**.
{% endstep %}

{% step %}
#### State Extraction

The major task in **state extraction** is to determine the **valid states** (e.g., those **reachable from the reset state**) among those identified by all possible **polarity assignments** to the **state variables**.&#x20;
{% endstep %}
{% endstepper %}

**Design systems for sequential optimization** leverage **optimization methods** in **both representation domains**.

#### Typical flow for Sequential Logic Optimization

The typical flow is summarized as follows:

1. FSM specification
2. State minimization
3. State encoding
4. Combinational (next-state, output) logic optimization
5. State extraction (for verification)
6. Retiming and synchronous transformations

And for each of the model we talked about just now, we will cover the following techniques to optimize it

* The State-based model
  * State minimization
  * State encoding
* The Structural model
  * Retiming
  * Synchronous/Network/RTL transformations

{% hint style="warning" %}
In the real design, I personally recommend to use the structural model to model a synchronous circuit and use the RTL transformation to optimize it. The state-based model is nothing but a normal FSM and will only be useful to model the control unit to generate the control signals. Try not to use FSM to model a datapath!
{% endhint %}

<details>

<summary>Not all sequential circuits are synchronous</summary>

* **Sequential Circuits** are the broad category (logic with memory).
* **Synchronous Circuits** are a specific _subset_ of sequential circuits.
* **Asynchronous** Circuits are the _other_ subset.

One example will be the [**latches**](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/sequential-logic-design/latches-and-flip-flops#d-latch), which are level-sensitive and are categorized to be **asynchronous circuits**.

</details>

[^1]: This means that all the FFs are clocked at the **same edge** from the **same clock source**.
