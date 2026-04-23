# Problem Set 2

## Problems

### 1. FSM State Minimization

<figure><img src="../.gitbook/assets/ps2-q1.png" alt=""><figcaption></figcaption></figure>

This problem is a classic state minimization problem. We need to master two techniques from this problem:

1. Do the proper state minimization from the state transition table
2. Draw the state transition graph from the state transition table

#### State Minimization

According to the three steps we mentioned in the [lecture](https://app.gitbook.com/s/W45nwClYZdzz9MQG1dUb/micheli/sequential-logic-optimization/synchronous-circuit-optimization-using-state-based-models#normal-method), which is

> * **Initial partition (**$$\Pi_1$$**):** States are placed in the same block if they produce identical **outputs** for every input.
> * **Refinement step (**$$\Pi_k\to\Pi_{k+1}$$**):** States remain in the same block if they were in the same block in  $$\Pi_k$$ **and** their next states fall in the same block of $$\Pi_k$$ for all inputs.
> * **Convergence:** The refinement process terminates when $$\Pi_{k+1}=\Pi_{k}$$.

The steps to solve this problem can be summarized as follows:

{% stepper %}
{% step %}
#### Do the initial partition based on the outputs

Based purely on the outputs, our initial partition will be

$$
\{A,B,C,D,E,F,H\},\{G\}
$$
{% endstep %}

{% step %}
#### Refine the initial partition

We first check each pair in the first block and see if their **next states** are in the same partition.

{% hint style="warning" %}
There is no need to check for the outputs as the initial partition is derived from the outputs! So, the states in the same block confirm will have the same outputs.
{% endhint %}

After checking, we find out that the states in the pair $$\{A,D\}$$  not **equivalent**. Thus, we need to separate state $$D$$ out from the first block to form the third block.

{% hint style="danger" %}
When separating the state $$D$$ out, we must also check if there are other states which are equivalent to $$D$$, meaning that they should have the **same outputs** and the **same next states**. Here, there is no such state, so $$D$$ itself will form the third block.
{% endhint %}

Now, our partition becomes

$$
\{A,B,C,E,F,H\},\{G\},\{D\}
$$

Start from scratch to check the first block, we find out that the states in the pair $$\{A,B\}$$ are not **equivalent**! Thus, $$B$$ needs to be separated out.

{% hint style="danger" %}
Pay attention, this time, there exists another state which is equivalent to state $$B$$ and this state is state $$F$$. Thus, both of the states $$B$$ and $$F$$ must be separated out to form the fourth block in our partition.
{% endhint %}

Now, our partition becomes

$$
\{A,C,E,H\},\{G\},\{D\},\{B,F\}
$$

Start from scratch again to do the pair-wise checking, we find out that the states in the pair $$\{A,H\}$$ doesn't match. Thus, state $$H$$ should be taken out and since there is no other state that is equivalent to $$H$$, our partition becomes

$$
\{A,C,E\},\{G\},\{D\},\{B,F\},\{H\}
$$

This is our final partition as all the pairs in the first block match! Thus, after doing state minimizations, we have 4 states.
{% endstep %}
{% endstepper %}

After doing the state minimization, our new state transition table now becomes as follows:

| Input | Current | Next | Output |
| ----- | ------- | ---- | ------ |
| 0     | A       | B    | 0      |
| 1     | A       | A    | 0      |
| 0     | B       | D    | 0      |
| 1     | B       | A    | 0      |
| 0     | D       | H    | 0      |
| 1     | D       | G    | 0      |
| 0     | G       | B    | 1      |
| 1     | G       | A    | 0      |
| 0     | H       | H    | 0      |
| 1     | H       | A    | 0      |

{% hint style="warning" %}
The trick here is that I replace state $$C$$ and $$E$$ with state $$A$$ as well as replace state $$F$$ with state $$B$$.
{% endhint %}

#### State Transition Graph Drawing

As state transition graph drawing is **always** based on the state transition table, after we get the state transition table after the state minimization above, the drawing of the state transition graph will become trivial, which can be shown as follows:

<figure><img src="../.gitbook/assets/ps2-q1-state-transition-diagram.svg" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
As we are dealing with a mealy machine here, so we will use the [mealy-machine drawing convention](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-02-digital-system-design-and-verilog#finite-state-machines) in our state transition graph!
{% endhint %}

### 2. FSM Back Engineering

1. For the questions (a), pay close attention to the structure of the table!
2. To minimize the area of a circuit, we should think of the skills like dynamic programming learned in [lec-08-technology-mapping.md](../lec/lec-08-technology-mapping.md "mention"). So, the systemetic approach is
   1. Decomposition the circuit using the standard cells
   2. Partitioning
   3. Do the pattern matching
   4. Do the covering and use dynamic programming to find the minimum cost of area.

### 3. Retiming

1. When there are two registers coming out from a node, in reality only **one register** is needed (We can add one more wire!)
