# Lec 02b - RTL Transformations

## Foundations of Synchronous Systems

### Data-Flow Graphs

A **Data-Flow Graph** (DFG) is a mathematical map of our hardware circuit. It doesn't show _what_ the data is (like "5" or "10"), but _how_ the data moves and where it gets processed.

* Graph ($$G$$): The entire circuit.
* Vertices ($$V$$): The processing units (Combinational Logic).
* Edges ($$E$$): The wires connecting them.

For example, in the following diagram, we can see three distinct elements. Each has a specific hardware meaning.

<figure><img src="../../.gitbook/assets/dfg-mac-example.png" alt=""><figcaption></figcaption></figure>

* **Nodes (Circles/Ovals)**:
  * **Meaning**: These are the operators (Adders, Multipliers, Logic Gates).
  * **Label** ($$d(v)$$): The number written next to or inside the node. This is the **Combinational Delay** (Calculation Time).
  * _Example:_ If a node has a "10" next to it, it takes 10ns to finish its job.
* **Edges (Arrows)**:
  * **Meaning**: These are the paths data travels.
  * **Direction**: Data always flows one way (**Unidirectional**).
* **The "D" (or "2D", "3D")**:
  * **Meaning**: This stands for Delay, but specifically Sequential Delay (Registers/Flip-Flops).
  * Weight ($$w(e)$$): The number of registers on a wire.
  * _Example:_ A box labeled "$$D$$" is 1 Register (1 clock cycle delay). A box labeled "$$2D$$" is 2 Registers in a series. A plain wire has $$0D$$.

{% hint style="danger" %}
#### Two Delays

We have seen two delays up to now and don't confuse with them!

* **Node Delay** ($$d$$): Time is consumed here. This limits how fast our frequency can be (Critical Path). This is called **combinational delay**.
  * _Analogy:_ The time it takes a chef to cook a burger.
* **Edge Delay/Weight** ($$w$$): Time is "paused" here. The data stops and waits for the next clock tick. This is called the **sequential delay**.
  * _Analogy:_ The customer waiting in line.
{% endhint %}

#### DFG on Single Rate System

The systematic procedure to draw DFG on a single rate system is as follows:

1. **For each input**, create one node that generates such input (to avoid floating edges, being a graph)
2. **For each output**, create one node that absorbs such output (to avoid floating edges)
3. Replace each **operator** with **node** with related **combinational delay**
4. Replace each **connection** with edge and related **sequential delay**

For example,

<figure><img src="../../.gitbook/assets/dfg-single-rate-conversion-example.png" alt=""><figcaption></figcaption></figure>

#### DFG on Multi-Rate System

In a multi-rate system, different signals are synchronized to clocks with different frequency. For example, in the following multi-rate system,

<figure><img src="../../.gitbook/assets/multi-rate-system-example.png" alt=""><figcaption></figcaption></figure>

* A has input frequency f<sub>A</sub>, outputs frequency 3f<sub>A</sub>.
* B has input frequency 5f<sub>B</sub>, outputs frequency 2f<sub>B</sub>.
* C has input frequency 3f<sub>C</sub>, outputs frequency 2f<sub>C</sub>.

We can derive the relationship between f<sub>A</sub>, f<sub>B</sub> and f<sub>C</sub> from the two connection wires:

1. A -> B: 3f<sub>A</sub> = 5f<sub>B</sub>, so f<sub>B</sub> = (3/5) f<sub>A</sub>.
2. B -> C: 2f<sub>B</sub> = 3f<sub>C</sub>, so f<sub>C</sub> = (2/3)f<sub>B</sub> = (2/5) f<sub>A</sub>.

We can represent a multi-rate system as single-rate by using the following two techniques

1. Apply **unfolding** to each node of multi-rate system (See later)
2. Start from output and go backwards until inputs are reached.

<figure><img src="../../.gitbook/assets/multi-rate-system-dfg-example.png" alt=""><figcaption></figcaption></figure>

#### Assumptions

In our analysis, we made the following assumptions

{% stepper %}
{% step %}
#### Functionality vs. Latency

For any RTL transformation to be valid, it must preserve **Functionality**, but it does _not_ have to preserve Latency.

* **Functionality (Must Keep)**: For any input sequence, the output sequence values and order must remain exactly the same.
* **Latency (Can Change)**: The number of clock cycles it takes to produce the first valid output can increase.
  * _Example:_ Adding pipeline registers increases latency (results arrive later) but improves frequency (clock runs faster). This is a legal transformation.
{% endstep %}

{% step %}
#### Timing Model Simplifications

To perform mathematical optimization on the Data Flow Graph (DFG), we simplify the timing analysis:

* **Cycle-Based Timing**: We ignore sub-cycle analog behaviors. We only care that the signal is stable at the end of the clock cycle.
* **The Period Equation:** $$T_{CK} \approx \tau_{COMB,max}$$
  * We approximate the clock period as simply the **Maximum Combinational Delay** in the circuit, often ignoring small overheads like clock skew ($$t_{skew}$$) or setup time ($$ $_{setup} $$) during the initial algorithmic steps.
{% endstep %}

{% step %}
#### The "Ideal Hardware" Assumptions

All transformation algorithms (Retiming, Folding, Unfolding) rely on these four simplifications:

1. **Load Independence**: The delay of a logic gate (node) is constant, regardless of what it is connected to.
2. **Wire Delay is Negligible**: Rearranging the graph does not change the delay of the wires (edges).
3. **Input Independence**: A computation node takes the same amount of time regardless of the input values (e.g., adding $$0+0$$ takes the same time as $$123+456$$).
4. **No Stalls**: The pipeline flows continuously; we ignore complex control hazards or memory waits.
{% endstep %}

{% step %}
#### Summary of Trade-offs

We apply the following transformations to shift the design point on the **Area-Speed-Power** triangle:

| Transformation | Area               | Throughput (Speed) | Latency           |
| -------------- | ------------------ | ------------------ | ----------------- |
| Parallelism    | ⬆️ Increase        | ⬆️ Increase        | ➖ Same            |
| Pipelining     | ↗️ Slight Increase | ⬆️ Increase        | ⬆️ Increase       |
| Folding        | ⬇️ Decrease        | ⬇️ Decrease        | ➖ Same            |
| Retiming       | ➖ Same (≈)         | ⬆️ Increase        | ➖ Same / Variable |
{% endstep %}
{% endstepper %}

#### Types of Synchronous Paths

A **Path** ($$p$$) is a sequence of connected nodes and edges starting from node $$ $u$ $$ and ending at node $$v$$. It can be denoted as $$u \to \dots \to v$$. We have the following three types of paths

{% stepper %}
{% step %}
#### Linear Pipeline

Data flows from one stage to the immediately next one.

<figure><img src="../../.gitbook/assets/linear-pipeline.png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Feedforward Path

Some data skips registers

<figure><img src="../../.gitbook/assets/feedforward-path.png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Feedback Path

Some data goes back to previous register.

<figure><img src="../../.gitbook/assets/feedback-path.png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

### Iteration Bound

There is a fundamental difference in how we optimize timing for recursive vs. non-recursive graphs (DFGs).

{% stepper %}
{% step %}
#### Non-recursive DFGs

In non-recursive DFGs, the data flows in one direction only, so we can achieve _any_ desired clock cycle time by inserting more pipeline registers (pipelining) and redistributing them (retiming).

The result is that speed is limited only by technology constraints (setup/hold times), not by the logic structure itself.
{% endstep %}

{% step %}
#### Recursive DFGs

Recursive DFGs contain feedback paths where outputs affect future inputs. For example, consider the following recurrence

```
y[n] = y[n−1] + x[n]
```

In this case, we typically cannot insert new registers into a loop without changing the algorithm's functionality. Remember that adding one register in the loop means "Wait **one clock cycle** before feeding the result back". So,

* **1 register** -> feedback after 1 cycle
* **2 registers** -> feedback after 2 cycles
* **k registers** -> feedback after k cycles

Let's say we insert **one register** in the loop with the recurrence relation above, our recurrence relation will become

```
y[n] = y[n−2] + x[n]
```

This is a **different algorithm**! In a nuthell, the result is that there is a fundamental "Speed Limit" (minimum cycle time) determined by the loops themselves. This speed limit is called **loop bound.**
{% endstep %}
{% endstepper %}

#### Loop Bound

The **Loop Bound** represents the minimum cycle time imposed by a single, specific loop. It assumes the total logic delay in the loop is evenly distributed across the available registers. It can be calculated using the following formula:

$$
\text{LoopBound} = \frac{t_{loop}}{w_{loop}}
$$

* $$t_{loop}$$: The sum of all combinational logic delays in that specific loop (computation time).
* $$w_{loop}$$: The number of delay elements (registers) in that specific loop.

For example, in the following recursive DFG, we can calculate the loop bounds for two existing loops.

<figure><img src="../../.gitbook/assets/loop-bound-calculation-example.png" alt="" width="563"><figcaption></figcaption></figure>

1. Loop 1 is the inner loop at the right side, containing 6 nodes and a total combinational delay of 12.
   1. Its loop bound is $$12\div(6-2)=3$$
2. Loop 2 is the big outer loop, containing 4 nodes and a total combinational delay of 4.
   1. Its loop bound is $$4\div(4-2)=2$$

After talking about the **loop bound**, we can see what the **iteration bound** is.

#### Iteration Bound

The Iteration Bound is the **critical path** of recursive systems. It defines the absolute minimum clock period achievable for the entire system, assuming ideal retiming (perfectly balanced stages). It is defined as:

$$
T_{\infty} = \max_{\text{all loops}} \left( \frac{t_{loop}}{w_{loop}} \right)
$$

* It is the **maximum** of all loop bounds in the DFG.
* If our target clock period $$T_{clk} < T_{\infty}$$, the design is impossible to implement without changing the algorithm itself.
* Formula for Minimum Cycle Time ($$T_{CK}$$): $$T_{CK} = \tau_{COMB, max} + t_{OH} \approx \frac{t_{loop}}{w_{loop}}$$
  * Assuming $$t_{OH}$$ is **negligible**

#### A Microprocessor Example

In Intel Itanium processor, the six loops limit the microprocessor's clock frequency.

<figure><img src="../../.gitbook/assets/iteration-bound-example.png" alt="" width="563"><figcaption></figcaption></figure>

In each ALU, we can see there is a loop with one register. To calculate the iteration bound, we can use the extra information on the combinational delay give below:

<figure><img src="../../.gitbook/assets/iteration-bound-example-extra-info.png" alt=""><figcaption></figcaption></figure>

Using the formula we have seen above, we can know that the iteration bound is

$$
T_{\infty}=\text{loopbound}_{\text{loop}}=\frac{590+60+100+80+100+70+\textcolor{red}{62}+\textcolor{red}{90}}{1}=1152\text{ps}
$$

{% hint style="danger" %}
If you have **multiple registers** appearing in the loop, let's say n. Then you need to add $$n \times t_{OH}$$ in your $$t_{loop}$$ term.
{% endhint %}

## Retiming

### Concepts and Properties

### Cut-Set Retiming

### Algorithms for Minimization

## Throughput Optimization

### Parallelism

### Pipelining

## Efficiency and Utilization

### Time Interleaving

### Transformation Comparisons
