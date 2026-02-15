# Strategies for Architectural Optimization

**Architectural optimization** comprises **scheduling** and **binding**. Complete architectural optimization is applicable to circuits that can be modeled by **sequencing graphs** (or equivalent representations) without a **start time** or **binding annotation**. Thus, the goal of architectural optimization is to determine a **scheduled sequencing graph** with a complete **resource binding** that satisfies the given **constraints** and optimizes some **figure of merit**.

<details>

<summary>Architectural Synthesis vs. Architectural Optimization</summary>

While **Architectural Synthesis** is the overarching process of mapping behavioral descriptions to structure, **Scheduling** and **Binding** are the specific mechanisms that perform **Architectural Optimization** by navigating the design space to determine the best trade-offs (Pareto points) between area and latency.

</details>

> TODO: Ask scheduling and binding are architectural optimization or architectural synthesis?

{% hint style="warning" %}
It is obvious that any **circuit model** described in terms of a **scheduled** and **bound sequencing graph** does not require any **optimization**, because the desired **design point** in the **design space** is already **prescribed**.
{% endhint %}

**Architectural optimization** consists of determining a **schedule** $$\phi$$ and a **binding** $$\beta$$ that optimize the objectives (**area**, **latency**, and **cycle time**). The optimization of these **multiple objectives** reduces to the computation of the [**Pareto points**](../introduction/computer-aided-synthesis-and-optimization.md#pareto-point) in the [**design space**](../introduction/computer-aided-synthesis-and-optimization.md#design-space) and to the evaluation (or estimation) of the corresponding **objective functions**.

**Architectural exploration** is often performed by the following three approaches

1. examining the **(area, latency) trade-off** for different values of the **cycle time**.
   1. Example is **scheduling**
2. searching for the **(cycle time, latency) trade-off** for a given **binding** (fixed area), or
   1. Example is **chaining**
3. searching for the **(area, cycle time) trade-off** for a given **schedule** (fixed latency).
   1. Example is **retiming**
