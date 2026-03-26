# Problem Set 1

## Problems

### 01. Netlist and Logic Network

#### Netlist

1. **Netlist** is used to represent a **structure** which is nothing but a bunch of blocks[^1] and their interconnections.
2. To represent a **structure**, we can use the following methods:
   1. module-oriented netlist.
   2. net-oriented netlist.
   3. Adjacency matrix where the column is the module and the row is the net.
   4. Hypergraph
   5. Bipartite graph

#### Logic Network

1. A **logic network** is nothing but **one block**, which has **multiple/one inputs** and **only one output**.
2. A **logic network graph**, is to implement that **logic network** with one big block using [smaller blocks](#user-content-fn-2)[^2].
   1. A **synchronous logic network graph** would be just to add the **registers** on the **edges** of a normal logic network graph.

### 02. Scheduling

1. One good way to do the scheduling problem is to draw a table whose column represents the clock cycle and the row represents the resouces.

### 03. Binding

1. Binding problem becomes complex when we deal with the registers sharing.

### 04. Combine Everything

In short, all the things we have seen here covers most of the [lec-05-microarchitecture-design.md](../lec/lec-05-microarchitecture-design.md "mention") content. This thing can only be done either by HLS tolls or by humans.

{% hint style="info" %}
Unlike the HLS synthesis used in the question, the logic synthesis will only do the boolean algebra optimization.
{% endhint %}

#### Draw the CDFG

The vertice represents the operator type, and the inputs coming to the vertices are the signals. For complex CDFGs like conditional, loops and function calling, the drawing might become more complex.

#### Scheduling

When we have two vertices which we are not sure whether which one can be issued first, look at their distance to the sink node, whichever has a shorter distance should be issued first.

> TODO: check the above point with prof.

In scheduling, if the constraint is resources, we should aim for **lowest latency**. If the constraint is **latency**, we should aim for **minimum resources** used.

#### Binding

To illustrate the binding function for the function units. Just draw a binding function and vertice table.

#### Register Sharing

1. Think about each register should be kept for how long.
2. The output signals **automatically** has a register that can be shared with others if possible.
3. The intuition is that we are **labeling** the intermediate signals in the CDFG with the shared registers.

#### Control Unit Synthesis

If an operator, like multiplier, expands for two cycles, the multiplexer selection unit should be don't care for the first cycle and technically the write enable signal can also be don't care in the first cycle.

#### Control Unit Optimization

Some techniques available reduce the row width, a.k.a, control-store word size, of the controlled unit implemented using microprogramming.

1. Combine the identical columns.
2. Combine the mutually exclusively columns and remember to reserve a binary representation for the NOP.
3. Combine the columns with an explicit shifted pattern and by doing rewiring on the other multiplexer(s).
4. Simplify the available ALU controls.

## Tips

[^1]: If you think of them as **logic gates**, life will be a lot easier. But, just keep in mind, a block doesn't need to be exactly **one** logic gate. Later you will see that a **block** is nothing but an operator with **multiple/one input** and **only one output.**

[^2]: The smaller blocks can be the fundamental logic gates.
