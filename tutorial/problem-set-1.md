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

## Tips

[^1]: If you think of them as **logic gates**, life will be a lot easier. But, just keep in mind, a block doesn't need to be exactly **one** logic gate. Later you will see that a **block** is nothing but an operator with **multiple/one input** and **only one output.**

[^2]: The smaller blocks can be the fundamental logic gates.
