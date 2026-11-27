# Computer-aided Synthesis and Optimization

**Computer-aided tools** provide an effective means for designing microelectronic circuits that are economically viable products.

* **Synthesis** techniques speed up the design cycle and reduce human effort.
* **Optimization** techniques enhance the design quality.

At present, **synthesis** and **optimization techniques** are used for most digital circuit designs. Nevertheless, their power is not yet exploited in full. It is one of the purposes of the book to f**oster the use of synthesis and optimization techniques**, because they can be instrumental in the progress of electronic design.

## Circuit Models

### Abstraction

A **model of a circuit** is an **abstraction**, i.e., a **representation** that shows **relevant features** without **associated details**. **Synthesis** is the **generation** of a **circuit model**, starting from a **less detailed one**. **Models** can be classified in terms of **levels of abstraction** and **views**. We consider here **three main abstractions**, namely: **architectural**, **logic**, and **geometrical**.

#### Architectural

At the **architectural level**, a **circuit** performs a set of **operations**, such as **data computation** or **transfer**.

{% hint style="success" %}
Examples of representations for architectural models: **HDL models**, **flow diagrams**
{% endhint %}

#### Logical

At the **logic level**, a **digital circuit** evaluates a set of **logic functions**.

{% hint style="success" %}
Examples of representations for logic models are **state transition diagrams**, **schematics**
{% endhint %}

#### Geometric

At the **geometrical level**, a **circuit** is a set of **geometrical entities**.

{% hint style="success" %}
Examples of representations for geometric models are **floor plans**, **layouts**.
{% endhint %}

***

A simple example of the different **modeling levels** of a simple processor is shown in **Figure 1.8**.

<figure><img src="../../.gitbook/assets/circuit-model-abstraction (1).png" alt="" width="282"><figcaption><p>Figure 1.8 Three abstraction levels of a circuit representation</p></figcaption></figure>

* At the **architectural level**, a **processor** is described by an **HDL model**.
* A **schematic** captures the **logic-level specification**.
* A **two-dimensional geometric picture** represents the **mask layout**.

Design consists of **refining** the abstract specification of the **architectural model** into the **detailed geometrical-level model**, that has enough information for the manufacturing of the **circuit**.

### View

We consider now the **views** of a model. They are classified as: **behavioral**, **structural**, and **physical**.

* **Behavioral views** describe the **function** of the **circuit** regardless of its **implementation**.
* **Structural views** describe a **model** as an **interconnection of components**.
* **Physical views** relate to the **physical objects** (e.g., **transistors**) of a **design**.

Models at different levels can be seen under different **views**. For example, at the **architectural level**,

* a **behavioral view** of a **circuit** is a set of **operations** and their **dependencies**.
* a **structural view** is an **interconnection** of the major **building blocks**.

As another example, consider the **logic-level model** of a synchronous circuit.

* A **behavioral view** of the **circuit** may be given by a **state transition diagram**, while
* its **structural view** is an **interconnection of logic gates**.

**Levels of abstractions** and **views** are **synthetically represented** by Figure 1.9, where **views** are shown as the **segments of the letter "Y"**.

<figure><img src="../../.gitbook/assets/circuit-model-view (1).png" alt="" width="563"><figcaption><p>Figure 1.9 Circuit views and levels of abstractions</p></figcaption></figure>

Consider again the **circuit** of example shown in Figure 1.8 above. Figure 1.10 highlights **behavioral** and **structural views** at both the **architectural** and the **logic levels**.

<figure><img src="../../.gitbook/assets/level-of-abstraction-views (1).png" alt="" width="563"><figcaption><p>Figure 1.10 Levels of abstraction and corresponding views</p></figcaption></figure>

## Synthesis

The model classification relates to a taxonomy of the synthesis tasks. **Synthesis** can be seen as a set of **transformations** between two **axial views**. In particular, we can distinguish the **synthesis subtasks** at the different **modeling levels** as follows:

*   **Architectural-level synthesis** consists of generating a **structural** view of an architectural-level model. This corresponds to

    * determining an assignment of the circuit functions to operators (called **resources**), as well as
    * their **interconnection** and the **timing** of their **execution**.

    It has also been called **high-level synthesis** or **structural synthesis**, because it determines the macroscopic (block-level) structure of the **circuit**. To avoid ambiguity, and for uniformity, we shall call it architectural synthesis.
* **Logic-level synthesis** is the task of **generating a structural view** of a **logic-level model**. **Logic synthesis** is the manipulation of logic specifications to create logic models as an interconnection of logic primitives. Thus, **logic synthesis** determines the **microscopic (gate-level) structure** of a circuit. The task of transforming a logic model into an interconnection of instances of library cells **—** i.e., the back end of logic synthesis **—** is often referred to as library binding or technology mapping.
* **Geometrical-level synthesis** consists of **creating a physical view** at the **geometric level**. It entails the **specification of all geometric patterns** defining the **physical layout** of the **chip**, as well as their **position**. It is often called **physical design**, and we shall call it so in the rest of this section.

The **synthesis tasks** are **synthetically depicted** in Figure 1.11.

<figure><img src="../../.gitbook/assets/synthesis-tasks (1).png" alt="" width="563"><figcaption><p>Figure 1.11 Levels of abstractions, views and synthesis tasks</p></figcaption></figure>

We now describe these tasks in more detail, considering them in the order corresponding to their use in a top-down synthesis system. This sequence is the **converse** of that corresponding to their historical development and level of maturity.

### Architectural Synthesis

A **behavioral architectural-level model** can be abstracted as a set of **operations** and **dependencies**. Architectural synthesis entails

1. identifying the **hardware resources** that can **implement the operations**,
2. **scheduling** the **execution time** of the operations, and
3. **binding them to the resources**.

In other words, **synthesis** defines a **structural model** of a **data path** as an **interconnection of resources**, and a **logic-level model** of a **control unit**, that issues the **control signals** to the **data path** according to the **schedule**.

{% hint style="danger" %}
The macroscopic figures of merit of the implementation, such as **circuit area** and **performance**, depend **heavily** on this step. (This is one of the NUS CG3207 Final questions in AY25/26 Sem 1)
{% endhint %}

Indeed, **architectural synthesis** determines the degree of parallelism of the operations. **Optimization** at this level is very important, as mentioned later in this section. **Architectural synthesis** is described in detail later in other section.

<details>

<summary>Example of Architectural Synthesis</summary>

We consider here first an example of a **behavioral view** of an **architectural model**. The example has been adapted from one proposed by Paulin and Knight. It models a circuit designed to solve numerically (by means of the forward Euler method) the following differential equation:

<p align="center"><span class="math">y'' + 3xy' + 3y = 0</span></p>

in the interval $$[0, a]$$ with step-size dx and initial values $$x(0) = x_0, y(0) = y_0, y'(0) = u_0$$.

***

The circuit can be represented by the following **HDL model**:

{% code lineNumbers="true" %}
```nginx
diffeq {
  read (x, y, u, dx, a);
  repeat {
    xi = x + dx;
    a1 = u - 3*x*u*dx + 3*y*dx;
    yl = y + u * dx;
    c = xi < a;
    x = xl; u = ul; y = yi;
    }
  until (c);
  write y;
}
```
{% endcode %}

For the architectural synthesis task, let us assume for simplicity that the data path of the circuit contains **two resources**: a **multiplier** and an ALU (which can perform **addition/subtraction** and **comparison**). The **circuit** also contains **registers**, **steering logic**, and a **control unit**.

{% hint style="success" %}
A **structural view** of the **circuit** at the **architectural level** shows the **macroscopic structure** of the **implementation**. This view can be described by a **block diagram**, as in Figure 1.12, or equivalently by means of a structural HDL.
{% endhint %}

<figure><img src="../../.gitbook/assets/architectural-synthesis-example (1).png" alt=""><figcaption><p>Figure 1.12 Example of structural view at the architectural level</p></figcaption></figure>

</details>

### Logic Synthesis

A **logic-level model** of a circuit can be provided by a **state transition diagram** of a **finite-state machine**, by a circuit schematic, or equivalently by an HDL model. It may be **specified by a designer** or synthesized from an architectural-level model.

The logic synthesis tasks may vary according to the nature of the circuit (e.g., sequential or combinational) and the starting representation (e.g., state diagram or schematic). The **p**ossible configurations of a **circuit** are many. **Optimization** plays a **major role**, in connection with **synthesis**, in determining the **microscopic figures of merit** of the **implementation**, as mentioned in later in this section.

The final outcome of logic synthesis is a fully **structural representation**, such as a **gate-level netlist**. Logic synthesis is described in detail later in other section.

<details>

<summary>Example of Logic Synthesis</summary>

Consider the **control unit** of the previous **circuit**. Its task is to **sequence the operations** in the **data path** by providing **appropriate signals** to the **resources**. This is achieved by **steering the data** to **resources** and **registers** in the block called "**Steering & Memory**" (Figure 1.12).

A **behavioral view** of the **control unit** at the **logic level** is given by a **transition diagram**, sketched in **Figure 1.13(a)**, where the **signals** to the "**Steering & Memory**" block are not shown for simplicity. The **control unit** uses:

* **one state** for **reading the data** (**reset state s₀**),
* **one state** for **writing the data** (**s₉**), and
* **seven states** for **executing the loop**.
* Signal **r** is a **reset signal**.

A **structural view** of the **control unit** at the **logic level** is shown by the **hierarchical schematic** of **Figure 1.13(b)**, which shows the **logic gates** that implement the **transitions among the states** (**s**<sub>**0**</sub>**, s**<sub>**1**</sub>**, …, s**<sub>**9**</sub>) and that **enable** the **reading** and **writing** of the **data**. The **subcircuit** that controls the **iteration** is represented by the box labeled "**loop control**."

<figure><img src="../../.gitbook/assets/logical-synthesis-example (1).png" alt=""><figcaption><p>Figure 1.13</p></figcaption></figure>

</details>

### Physical Design

**Physical design** consists of **generating the layout** of the **chip**. The **layers** of the **layout** correspond to the **masks** used for **chip fabrication**. Therefore, the **geometrical layout** is the **final target** of **microelectronic circuit design**.

**Physical design** depends heavily on the **design style**:

* For **custom design**, **physical design** is **handcrafted** using **layout editors**. This means the **designer** renounces the use of **automated synthesis tools** in order to **optimize the circuit geometries** by **fine hand-tuning**.
* For **prewired circuits**, **physical design** is performed in a **virtual fashion**, because **chips** are **fully manufactured in advance**. **Chip personalization** is instead done by a **fuse map** or a **memory map**.

The **major tasks** in **physical design** are **placement** and **wiring** (also called **routing**). **Cell generation** is essential in the case of **macro-cell design**, where **cells** are **synthesized** rather than **extracted from a library**.

## Optimization

**Circuit optimization** is often performed in conjunction with **synthesis**. In this **book**, we consider the **optimization** of two [**quality**](design-of-microelectronic-circuits.md#optimization) **measures**, namely, **area** and **performance**. We shall also comment on the **relation to testability** in the other sections.

### Area

**Circuit area** is an **extensive quantity**. It is measured by the **sum of the areas** of the **circuit components** and therefore can be **computed** from a **structural view** of a **circuit**, once the **areas of the components** are known. The **area computation** can be performed **hierarchically**. Usually, the **fundamental components** of **digital circuits** are **logic gates** and **registers**, whose **area** is known **a priori**. The **wiring area** often plays an **important role** and should not be **neglected**. It can be derived from a **complete physical view** or **estimated** from a **structural view** using **predictive** or **statistical models**.

### Performance

**Circuit performance** is an **intensive quantity**. It is not **additive**, and therefore its **computation** requires **analyzing** the **structure** and often the **behavior** of a **circuit**. To be more specific, we need to consider the **meaning of performance** in more detail, according to the different **classes** of **digital circuits**.

#### Combinational Circuit Performance

The **performance** of a **combinational logic circuit** is measured by the **input/output propagation delay**. Often, a **simplifying assumption** is made: all **inputs** are **available at the same time**, and the **performance** relates to the **delay** through the **critical path** of the **circuit**.

#### Synchronous Sequential Circuit Performance

The performance on the synchronous sequential circuit can be measured in **three** metrics:

1. Cycle-time
2. Latency
3. Throughput

{% hint style="danger" %}
Non-piplined synchronous sequential circuits usually care about **cycle-time** and **latency**, while pipelined synchronous sequential circuits care about **cycle-time, latency** and **throughput.**
{% endhint %}

{% stepper %}
{% step %}
**Cycle-time**

The **performance** of a **synchronous sequential circuit** can be measured by its **cycle-time**, i.e., the **period** of the **fastest clock** that can be applied to the **circuit**.

{% hint style="warning" %}
The **delay** through the **combinational component** of a **sequential circuit** is a **lower bound** on the **cycle-time**.
{% endhint %}
{% endstep %}

{% step %}
**Latency**

When considering an **architectural-level model** of a **circuit** as a **sequence of operations**, with a **synchronous sequential implementation**, one measure of **performance** is **circuit latency**, i.e., the **time required** to **execute the operations**.

**Latency** can be measured in terms of **clock cycles**. Thus, the **product** of the **cycle-time** and **latency** determines the **overall execution time**. Often, **cycle-time** and **latency** are **optimized independently**, for the sake of **simplifying the optimization problem** as well as **satisfying other design constraints**, such as **interfacing to other circuits**.

{% hint style="info" %}
NUS EE4415 also has some notes on [latency](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/part-1-lec-digital-design-flow/lec-1b-timing-synchronous#latency), which are quite good to refer to!
{% endhint %}
{% endstep %}

{% step %}
**Throughput**

> **Synchronous circuits** can implement a **sequence of operations** in a **pipeline fashion**, where the **circuit** performs **concurrent operations** on **different data sets**.

An additional measure of **performance** is the **rate** at which **data** are **produced** and **consumed**, called the **throughput** of the **circuit**. In other words, **throughput** is "number of computations completed per unit of time."

In a **non-pipelined circuit**, the **throughput** is less than (or equal to) the **inverse** of the **product** of the **cycle-time** and **latency**.

<details>

<summary>Why "inverse" here?</summary>

As latency is the number of clock cycles needed to complete **one computation**. The product of cycle-time and latency will be **the amount of time needed to complete one computation**. Mathematically speaking, its **inverse** is the **number of computations complete per unit of time**, which is exactly what **throughput** is.

</details>

**Pipelining** allows a **circuit** to **increase its throughput** beyond this limit. **Maximum-rate pipelining** occurs when the **throughput** is the **inverse of the cycle-time**, i.e., when **data** are **produced** and **consumed** at **each clock cycle**.

{% hint style="info" %}
NUS EE4415 also has some notes on [throughput](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/part-1-lec-digital-design-flow/lec-1b-timing-synchronous#throughput), which are quite good to refer to!
{% endhint %}
{% endstep %}
{% endstepper %}

According to these **definitions** and **models**, **performance optimization** consists in

1. **minimizing** the **delay** (for **combinational circuits**), the **cycle-time** and the **latency** (for **synchronous circuits**), and
2. **maximizing** the **throughput** (for **pipelined circuits**).

### Design Space

We consider here **design optimization** as the **combined minimization** of **area** and **maximization** of **performance**.

{% hint style="success" %}
This is the motivation for finding the pareto point which will be introduced later!
{% endhint %}

**Optimization** may be subject to **constraints**, such as **upper bounds** on **area** and **lower bounds** on **performance**. We can **abstract** the **optimization problem** as follows: the different **feasible** [**structural implementations**](#user-content-fn-1)[^1] of a **circuit** define its **design space**.

The **design space** is a **finite set of design points**. Associated with each **design point**, there are **values** of the **area** and **performance evaluation functions**. There are as many **evaluation functions** as the **design objectives** of interest, such as **area**, **latency**, **cycle-time**, and **throughput**. We call **design evaluation space** the **multidimensional space** spanned by these [**objectives**](#user-content-fn-2)[^2].

<details>

<summary>Example of Design Space</summary>

Let us construct a **simplified design space** for the previous example, considering only **architectural-level modeling**. For simplicity, we assume that **resources execute in one cycle** and the **delay** in **steering and control logic** is **negligible**, so **cycle-time** can be ignored.

The **design space** corresponds to structures with **a₁ multipliers** and **a₂ ALUs**, where **a₁, a₂** are positive integers. Examples include **(a**<sub>**1**</sub>**=1, a**<sub>**2**</sub>**=1)** and **(a**<sub>**1**</sub>**=2, a**<sub>**2**</sub>**=2)**.

Assume **area costs** as follows: **multiplier = 5 units**, **ALU = 1 unit**, and **control/steering/memory = 1 unit**. Then:

* The first implementation **(1,1)** has **area = 7** and **latency ∝ 7**.
* The second implementation **(2,2)** has **area = 13** and **latency ∝ 4**.

Considering the design points **(a**<sub>**1**</sub>**,a**<sub>**2**</sub>**) = (1,1), (1,2), (2,1), (2,2)**, the corresponding **design evaluation space** is shown in **Figure 1.15**, with numbers in parentheses indicating the associated points in the **design space**.

<figure><img src="../../.gitbook/assets/example-design-space (1).png" alt="" width="563"><figcaption><p>Figure 1.15 Example of design (evaluation) space</p></figcaption></figure>

</details>

### Pareto Point

**Circuit optimization** corresponds to **searching** for the **best design**, i.e., a **circuit configuration** that **optimizes** all **objectives**. Since our **optimization problem** involves **multiple criteria**, special attention must be given to defining **points of optimality**. For the sake of **simplicity** and **clarity**, and without loss of **generality**, we assume now that the **optimization problem** corresponds to a **minimization-one**. Note that **maximizing the throughput** corresponds to **minimizing** its **complement**.

A **point** of the **design space** is called a **Pareto point** if there is no other **point** in the **design space** with at least one **inferior objective**, all others being **inferior or equal**.

{% hint style="warning" %}
The above is the definition of a **pareto point**. In other words, one point is a **Pareto point** if there is no other design out there that beats it in one category without losing to it in another.
{% endhint %}

A **Pareto point** corresponds to a **global optimum** in a **monodimensional design evaluation space** and generalizes this concept to a **multidimensional context**. There may be **many Pareto points**, representing **design implementations** not **dominated** by others and hence **worth consideration**.

The **image** of the **Pareto points** in the **design evaluation space** is the set of **optimal trade-off points**, and their **interpolation** yields a **trade-off curve** or **surface**.

<details>

<summary>Example of Pareto Points</summary>

Consider the **design space** and the **design evaluation space** of the example [above](computer-aided-synthesis-and-optimization.md#example-of-design-space). The **point (1,2)** of the **design space** is **not a Pareto point**, because it is **dominated** by **point (1,1)**, which has **equal latency** and **smaller area**. In other words, **adding an ALU** to an implementation with **one multiplier** increases the **area** without reducing the **latency**, so this **circuit structure** can be **dropped** from consideration. The **remaining points** **(1,1), (2,1), (2,2)** are **Pareto points**. It can be shown that **no other pair** of **positive integers** is a **Pareto point**, as the **latency** cannot be **reduced further** by adding **additional resources**.

</details>

We now consider the examples of design (evaluation) spaces for different circuits classes.

#### Combinational Logic Circuits

In this case, the **design evaluation space objectives** are **(area, delay)**. **Figure 1.16** shows an example of the **trade-off points** in this **space** for **multiple-level logic circuit implementations**. These points can be **interpolated** to form an **area–delay trade-off curve**.

<figure><img src="../../.gitbook/assets/area-delay-design-space (1).png" alt="" width="527"><figcaption><p>Figure 1.16 Design (evaluation) space: area/delay trade-off points for a 64-bit adder. (Area and delay are measured in terms of equivalent gates)</p></figcaption></figure>

<details>

<summary>Example of Area/Delay trade-off</summary>

As a second example, consider the **implementation** of a **circuit** whose **logic behavior** is the **product of four Boolean variables** (e.g., (x = p q r s)). Assume the **implementation** is constrained to using **two-input** and **three-input AND gates**, whose **area** and **delay** are **proportional** to the **number of inputs**, and that the **inputs arrive simultaneously**. Elements of the **design space** are shown in **Figure 1.17**, representing **four different logic structures**.

The corresponding **design evaluation space** is shown in **Figure 1.18**. Note that the **structures** labeled **c** and **d** in **Figure 1.17** are **not Pareto points**.

<figure><img src="../../.gitbook/assets/area-latency-design-space (1).png" alt="" width="563"><figcaption><p>Figure 1.18 Example of design evaluation space: (a) and (b) are area/delay trade-off points</p></figcaption></figure>

</details>

#### Sequential Logic Circuits

When we consider **logic-level models** for **sequential synchronous circuits**, the **design evaluation space objectives** are **(area, cycle-time)**, where the **cycle-time** is **bounded from below** by the **critical path delay** in the **combinational logic component**. Therefore, the **design evaluation space** is similar in concept to that for **combinational circuits**, as shown in Figures 1.16 and [1.18](computer-aided-synthesis-and-optimization.md#example-of-area-delay-trade-off).

{% stepper %}
{% step %}
#### **Nonpipelined**

When considering **nonpipelined architectural-level models**, the **design evaluation space objectives** are **(area, latency, cycle-time)**, where the **product** of the last two yields the **overall execution delay**. An example of the **design evaluation space** is shown in **Figure 1.19**. **Slices** of the **design evaluation space** for specific values of one **objective** are also of interest; for example, the **(area, latency) trade-off** is shown in **Figure 1.15**.

<figure><img src="../../.gitbook/assets/area-latency-delay-trade-off (1).png" alt="" width="563"><figcaption><p>Figure 1.19 Design evaluation space: area/latency/cycle-time trade-off points</p></figcaption></figure>
{% endstep %}

{% step %}
#### **Pipelined**

For **architectural-level models** of **synchronous pipelined circuits**, **performance** is additionally measured by the **circuit throughput**. Hence, the **design evaluation space** involves the **quadruple** **(area, latency, cycle-time, throughput)**. **Architectural trade-offs** for **pipelined circuit implementations** will be described later in other section, with **examples** provided.
{% endstep %}
{% endstepper %}

#### General Approaches to Optimization

**Circuit optimization** involves **multiple objective functions**. In the previous examples, the **optimization goals** ranged from **two to four objectives**. The **optimization problem** is **difficult to solve** due to the **discontinuous nature** of the **objective functions** and the **discrete nature** of the **design space**, i.e., the set of **feasible circuit implementations**.

In general, **pareto points** are solutions to constrained optimization problems.

{% stepper %}
{% step %}
#### **Combinational Logic Optimization**

Consider, for example, **combinational logic optimization**. Two **problems** of interest are:

* **Minimize the circuit area** under **delay constraints**.
* **Minimize the circuit delay** under **area constraints**.

Unfortunately, due to the **difficulty** of the **optimization problems**, only **approximations** to the **Pareto points** can be **computed**.
{% endstep %}

{% step %}
#### **Synchronous Sequential Logic Optimization**

Consider next **architectural-level models** of **synchronous circuits**. **Pareto points** are **solutions** to the following **problems**, for different values of the **cycle-time**:

* **Minimize the circuit area** under **latency constraints**.
* **Minimize the circuit latency** under **area constraints**.

{% hint style="warning" %}
In a **pipelined** system, we are more interested in the **throughput** rather the **latency** of the system.
{% endhint %}

These two **problems** are often referred to as **scheduling problems**. Unfortunately, the **scheduling problems** are **hard to solve exactly** in most cases, and only **approximations** can be obtained. When considering **pipelined circuits**, **Pareto points** can be **computed** (or **approximated**) by considering different values of **cycle-time** and **throughput**, and by solving the corresponding **scheduling problems**.
{% endstep %}
{% endstepper %}

[^1]: Here, **structural implementations** refer to the **exact resources** that will be used to **build the circuit**. For example, a design point may specify using **one ALU and one multiplier**, while another may use **two ALUs and one multiplier**.

[^2]: Examples of "objectives" are area, latency, cycle-time and throughput.
