# Synchronous Circuit Optimization using State-Based Models

We consider in this section algorithms for sequential optimization using **state-based models** as well as transformations into and from **structural models**. We consider **Mealy-type finite-state machines**, defined by the quintuple $$X, Y, S, \delta, \lambda$$, as introduced earlier. We denote $$|S|$$ (the size of the states) by $$n_s$$, in the rest of this section.

> \[_From the earlier section_]: A **finite-state machine** can be described by:
>
> * A set of **primary input patterns**, $$X$$.
> * A set of **primary output patterns**, $$Y$$.
> * A set of **states**, $$S$$.
> * A **state transition function**, $$\delta: X \times S \to S$$.
> * An **output function**, $$\lambda: X \times S \to Y$$ for **Mealy models** or $$\lambda: S \to Y$$ for **Moore models**.
> * An **initial state**​.

## State Minimization

The **state minimization problem** aims at reducing the number of **machine states**. This leads to a reduction in the size of the **state transition graph**. **State reduction** may correlate to a reduction of the number of **storage elements**. (When states are **encoded** with a **minimum number of bits**, the number of **registers** is the **ceiling** of the logarithm of the number of states.) The reduction in **states** correlates to a reduction in **transitions**, and hence to a reduction of **logic gates**.

**State minimization** can be defined informally as deriving a **finite-state machine** with **similar behavior** and a **minimum number of states**. A more precise definition relies on choosing to consider **completely** (or **incompletely**) specified **finite-state machines**. This decision affects the **formalism**, the **problem complexity**, and the **algorithms**. Hence, **state minimization** is described separately for both cases in the following sections.

### Optimization for Completely Specified FSM

When considering **completely specified finite-state machines**, the **transition function** δ\deltaδ and the **output function** λ\lambdaλ are specified for each pair $$(\text{input}, \text{state}) \in X \times S$$. Two **states** are **equivalent** if the **output sequences** of the finite-state machine, initialized in the two states, **coincide for any input sequence**. Equivalency is checked by using the result of the following **theorem**.

> **Theorem 9.2.1.** Two **states** of a **finite-state machine** are **equivalent** if and only if, for any **input**, they have **identical outputs** and the corresponding **next states** are **equivalent**.

Now, we will introduce two methods to do the state optimization.

#### Normal Method

> TODO: Missing formal notation because the lack of the following maths from Discrete Maths
>
> 1. Symmetic, reflexive, transitive
> 2. Equivalence classes.

<details>

<summary>Example of using the normal method to minimize the states</summary>

<figure><img src="../../.gitbook/assets/state-minimization-normal-1.png" alt=""><figcaption><p>Figure 9.3 State diagram</p></figcaption></figure>

Consider the **state diagram** shown in **Figure 9.3**, whose **state table** is reported next:

<figure><img src="../../.gitbook/assets/state-table.png" alt="" width="560"><figcaption></figcaption></figure>

The **state set** can be **partitioned** first according to the **outputs**, i.e.,

<p align="center"><span class="math">\Pi_1 = \bigl\{ \{s_1, s_2\}, \{s_3, s_4\}, \{s_5\} \bigr\}</span></p>

Then we check each **block** of $$\Pi_1$$ to see if the corresponding **next states** are in a **single block** of $$\Pi_1$$ for any **input**. The **next states** of $$s_1$$ and $$s_2$$​ **match**. The **next states** of $$s_3$$ and $$s_4$$ are in **different blocks**. Hence, the **block** $$\{s_1, s_3, s_4\}$$ must be **split**, yielding:

<p align="center"><span class="math">\Pi_2 = \bigl\{ \{s_1, s_2\}, \{s_3\}, \{s_4\}, \{s_5\} \bigr\}</span></p>

When checking the **blocks** again, we find that no **further refinement** is possible, because the **next states** of $$s_1$$​ and $$s_2$$​ **match**. Hence, there are **four classes of compatible states**. We denote $$\{s_1, s_2\}$$ as $$s_{12}$$ in the following **minimal table**:

<figure><img src="../../.gitbook/assets/minimal-state-table.png" alt="" width="563"><figcaption></figcaption></figure>

The corresponding diagram is shown in Figure 9.4

<figure><img src="../../.gitbook/assets/minimal-state-diagram.png" alt=""><figcaption><p>Figure 9.4 Minimum-state diagram. (Dotted edges are superfluous)</p></figcaption></figure>

</details>

{% hint style="warning" %}
The complexity of this algorithm is $$O(n_s^2)$$.
{% endhint %}

#### Hopcroft's method

In the **algorithm** described above, the **refinement of the partitions** is done by looking at the **transitions** from the **states** in the **block under consideration** to other **states**. **Hopcroft** suggested a **partition refinement method** where the **transitions into the states** of the **block under consideration** are considered.

<details>

<summary>Example of using the Hopcroft method to minimize the states</summary>

Consider the **table** of [**Example above**](synchronous-circuit-optimization-using-state-based-models.md#example-of-using-the-normal-method-to-minimize-the-states). The **state set** can be **partitioned** first according to the **outputs**, as in the previous case.

<p align="center"><span class="math">\Pi_1 = \bigl\{ \{s_1, s_2\}, \{s_3, s_4\}, \{s_5\} \bigr\}</span></p>

Then we check each block of $$|\Pi_1|$$. Let $$A = \{s_5\}$$. Let us consider input 1. The states whose next state is $$s_5$$ are set $$P = \{s_1, s_2, s_5\}$$. Block $$A = \{s_1, s_2\}$$ is a subset of $$P$$ and requires no further split. Block $$B = \{s_1, s_4\}$$ is not a subset of $$P$$, and $$B \cap P = \{s_1\}$$. Hence, the block is split as $$\{\{s_1\}, \{s_4\}\}$$, yielding:

<p align="center"><span class="math">\Pi_2 = \bigl\{ \{s_1, s_2\}, \{s_3\}, \{s_4\}, \{s_5\} \bigr\}</span></p>

No further splits are possible and $$\Pi_2$$ defines our foru classes of equivalent states.

</details>

{% hint style="warning" %}
The complexity of this algorithm is $$O(n_s\log n_s)$$.
{% endhint %}
