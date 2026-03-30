# Problem Set 2

## Problems

### 1. FSM State Minimization

1. Whenever we separate a "state" from a set called $$A$$ into a new set called $$B$$, we should do the **pair-wise** check in set $$A$$ to see if there are any other states that need to be separated out. If so, those states will be in a new set called $$C$$.

### 2. FSM Back Engineering

1. For the questions (a), pay close attention to the structure of the table!
2. To minimize the area of a circuit, we should think of the skills like dynamic programming learned in [lec-08-technology-mapping.md](../lec/lec-08-technology-mapping.md "mention"). So, the systemetic approach is
   1. Decomposition the circuit using the standard cells
   2. Partitioning
   3. Do the pattern matching
   4. Do the covering and use dynamic programming to find the minimum cost of area.

### 3. Retiming

1. When there are two registers coming out from a node, in reality only **one register** is needed (We can add one more wire!)
