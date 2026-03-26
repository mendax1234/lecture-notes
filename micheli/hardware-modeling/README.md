# Hardware Modeling

A model of a circuit is an **abstraction**, i.e., a representation that shows relevant features without associated details. **Models** are used to specify circuits, to reason about their properties and as means of transfering the information about a design among humans and humans as well as among humans and computer-aided design (CAD) tools.

<details>

<summary>What is a circuit specification?</summary>

Circuit specifications are **models** describing circuits to be implemented, which are often accompanied by **constraints** on the desired realization, e.g., performance requirements.

</details>

A circuit can be modeled differently according to&#x20;

1. the desired **abstraction level** (e.g., architectural, logic, geometric),
2. **view** (e.g., behavioral, structural, physical), and
3. the **modeling means** being used (e.g., language, diagram, mathematical model).

Abstraction levels and views were described in [computer-aided-synthesis-and-optimization.md](../introduction/computer-aided-synthesis-and-optimization.md "mention"). We comment now on the "**modeling means**".

{% hint style="warning" %}
This book does not advocate the use of any specific hardware language. The synthesis techniques that we present have **general value** and are not related to the specifics of any particular language. For this reason, we shall consider also **abstract models** for circuits at both the **architectural** and **logic levels**.
{% endhint %}

**Abstract models** are **mathematical models** based on **graphs** and **Boolean algebra**.

* They are **powerful** enough to capture the **essential features described by HDL and diagram models**.
* At the same time, they are **simple** enough that properties of circuit transformations can be proven.

{% hint style="warning" %}
Perhaps the two bullet points above are the most important **intuitions** from this section.
{% endhint %}

Figure 1.20 (we have seen this at the [..](../../ "mention") chapter) shows the relations among HDL models, abstract models, and the synthesis tasks.

<figure><img src="../../.gitbook/assets/circuit-models-synthesis-optimization.png" alt=""><figcaption><p>Figure 1.20 Circuit models, synthesis and optimization: a simplified view</p></figcaption></figure>

The **language models** can be thought of as a way to specify the specifications. In other words, what we want to achieve. In HDL, we have two flavors: **normal RTL** and **Netlist**. Thus, we have different **abstract models** to capture the information described by these two flavors of HDL.

{% stepper %}
{% step %}
#### Normal RTL

This normal RTL corresponds to the first two rows which are under the **behavioral view**. In the normal RTL, we can write both **data paths** and **control units**.

* For **data paths**, we can use the following abstract models:
  * [Data-flow and Sequencing Graphs](abstract-models.md#data-flow-and-sequencing-graphs).
  * [Logic networks and Logic network Graphs](abstract-models.md#logic-networks).
  * For purely combinational data path, we can even use [mapped networks](abstract-models.md#mapped-network).
* For **control units**, we can use the following abstract models:
  * [State-transition diagrams](abstract-models.md#state-diagrams)
{% endstep %}

{% step %}
#### Netlist HDL

The netlist which is the output of the logic synthesis is nothing but a bunch of standard cell modules instantiated in the HDL. Thus, it is under the **structural view** of the graph above. For this kind of HDL, the abstract models are:

* [Structure](abstract-models.md#structure), like Incidence matrix/netlist, hypergraph, bipartite graph.
* [Logic networks and Logic network Graphs](abstract-models.md#logic-networks).
{% endstep %}
{% endstepper %}
