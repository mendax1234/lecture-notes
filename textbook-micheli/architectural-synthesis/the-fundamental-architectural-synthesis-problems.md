# The Fundamental Architectural Synthesis Problems

We now consider the fundamental **problems** in **architectural synthesis** and **optimization**. We assume that a **circuit** is specified by:

* A **sequencing graph**.
* A set of **functional resources**, fully characterized in terms of **area** and **execution delays**.
* A set of **constraints**.

**Architectural synthesis** and **optimization** consists of two **stages**.

1. First, **placing the operations in time and space**, i.e., determining the **time interval** for their **execution** and their **binding to resources**.
2. Second, determining the detailed **interconnections** of the **data path** and the **logic-level specifications** of the **control unit**.

We now show that the first **stage** is equivalent to **annotating** the **sequencing graph** with additional **information**.

{% hint style="warning" %}
Usually, **scheduling** is done first and then is **binding**.
{% endhint %}

## The Temporal Domain: Scheduling

**Scheduling** is the task of

1. Associating a **start-time** with each operation
2. Determining **latency** and parallelism of the implementation

To formally define what the **scheduling** is, let's make some notation conventions.

1. We denote the **vertices** in the sequencing graphs as a set $$V=\{v_i,i=0,1,\dots,n\}$$. Each vertex represents one operation.
2. We denote the **execution delays** of the operations by the set $$D=\{d_i,i=0,1,\dots,n\}$$. The unit here is **clock cycles**.
3. We define the **start time** of an operation as the time at which the operation starts its execution. It is represented by the set $$T=\{t_i,i=0,1,\dots,n\}$$.
4. Using the above three definitions, we can find out that the **latency**, which is denoted by $$\lambda$$, is just $$\lambda=t_n-t_0$$.

Now, let's give a **formal definition** of **schedule**:

> A **schedule** of a sequencing graph is a function $$\phi:V\to Z^+$$, where $$\phi(v_i)=t_i$$ denotes the operation start time such that $$t_i\geq t_j+d_j$$, $$\forall(i,j):(v_i,v_j)\in E$$. ($$t_i\geq t_j+d_j$$ is called the **precedence constraint**)

Thus, a more formal definition of **scheduling** will be

> **Scheduling** is thte task of determining the start times, subject to **precedence constraints** specified by the sequencing graph.

To get a glimpse of scheduling, let's see some examples

### Normal Scheduling

Here, we will look at two examples of scheduling under no constraint and constraints.

#### Example of Unconstrained Scheduling

In the **unconstrained scheduling**, which means any number of any type of resources is allowed to use, the scheduling can be done as follows:

<figure><picture><source srcset="../../.gitbook/assets/unconstrained-scheduling-dark.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/unconstrained-scheduling-light.png" alt="" width="563"></picture><figcaption><p>Figure 4.3 Unconstrained scheduling sequence graph</p></figcaption></figure>

The latency of this schedule is $$\lambda=t_n-t_0=5-1=4$$.

{% hint style="danger" %}
The source ($$v_0$$) always starts at **cycle 1** while the sink ($$v_n$$) always starts at the last cycle + 1.
{% endhint %}

#### Example of Contrained Scheduling

If we use **dedicated binding**, which is to constrain to use 1 function unit per type (e.g., 1 adder and 1 multiplier). Our scheduling will look like as follows:

<figure><picture><source srcset="../../.gitbook/assets/constrained-scheduling-dark.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/constrained-scheduling-light.png" alt=""></picture><figcaption><p>Figure 4.4 Constrained scheduling sequenc graph</p></figcaption></figure>

In this case, our latency is $$\lambda=t_n-t_0=8-1=7$$

### Scheduling with Chaining

The **scheduling formulation** can be extended by considering the **propagation delays** of the **combinational resources** instead of the **integer execution delays**. Thus, two (or more) **combinational operations** in a **sequence** can be **chained** in the same **execution cycle** if their overall **propagation delay** does not exceed the **cycle-time**. This approach can be further extended to **chains of resources** whose overall **delay** spans more than one **cycle**.

**Scheduling with chaining** can provide **tighter schedules** in some cases, but **constrained schedules** are harder to **compute**.

#### Example of Scheduling with Chaining

Let's assume our multiplication takes 35ns and others take 25ns and our cycle time is 50ns. We can **chain** the operation 4 and 5 in Figure 4.3 to happen in one clock cycle! This will give us a sequence graph shown as follows:

<figure><picture><source srcset="../../.gitbook/assets/schedule-chain-dark.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/schedule-chain-light.png" alt=""></picture><figcaption><p>Schedule with chaining sequence graph</p></figcaption></figure>

In this case, we reduce our latency to 3 as now $$\lambda=t_n-t_0=4-1=3$$.
