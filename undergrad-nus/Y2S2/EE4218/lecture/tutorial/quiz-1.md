# Quiz 1

## Problems

### 1. Area Estimation

**Question**: The preliminary non-trivial area estimate obtained from a non-scheduled sequencing graph is a

* [x] worst-case estimate
* [ ] best-case estimate

***

**Sol**. This is a **worst-case estimate** in terms of area. We've seen this in the [lecture](https://app.gitbook.com/s/W45nwClYZdzz9MQG1dUb/micheli/hardware-modeling/abstract-models#vertex-attributes-and-estimates) on abstract models!

### 2. Loop Unrolling

**Question**: Loop expansion / unrolling generally helps reduce the hardware required to accelerate a set of iterative computations.

* [ ] True
* [x] False

***

**Sol**. It is obviously **false**.

### 3. Pareto Points

**Question**: Points A and B correspond to the only two feasible implementations in the design space while exploring the area/latency trade-off of a certain behavioural specification, for a specified clock rate. Point A has a higher area and higher latency than point B. Hence, point B is a non-dominated point.

* [x] True
* [ ] False

***

**Sol**. The answer is **true**. Actually, this is an English problem. Based on the question, point B is obviously a pareto point. We can also say that point A is **dominated** by point B, which is equivalent to saying that, "A is dominated and B is non-dominated."

### 4. HDL Basics

**Question**: For the HDL code of a sequential circuit to be synthesizable, it is necessary to assign a value to every output for every possible input combination.

* [ ] True
* [x] False

***

**Sol.** This is obviously **false**. This requirement is only for **combinational** circuits. For sequential circuits, there is no such requirement.

### 5. Binding

**Question**: All the information regarding the schedule can be obtained from the binding function, assuming that the binding is done after scheduling.

* [ ] True
* [x] False

***

**Sol**. This is **false**. Purely from the binding function, the only thing we can know is that, if they are bounded to the same resource, they won't be executed together. That's all what we know, which only gives us some **partial information** about the scheduling.

### 6. BRAM and Distributed RAM

**Question**:

1. Statement A : Dedicated (Block) RAM can be read only synchronously.
2. Statement B : Distributed RAM can be read asynchronously.

Which one of the following option is correct:

* [ ] Statement A is True, Statement B is false.
* [ ] Statement B is True, Statement A is false.
* [x] Both statements are True.
* [ ] Both statements are False.

***

**Sol**. The answer is both are **true**. It is trivial though.

### 7. Non resource-dominated circuits

**Question**: A microcontroller is generally NOT a resource-dominated circuit.

* [x] True
* [ ] False

***

**Sol**. This is obviously **true**.

### 8. Area & Resource Estimation

**Question**: Area and performance estimation is much harder for a resource-dominated circuit as compared to a non-resource-dominated circuit.

* [ ] True
* [x] False

***

**Sol**. The answer is obviously **false**.

### 9. Microarchitecture Design

**Question**: The expressions below have to be accelerated using a hardware accelerator.

```c
x = a * b * c + 3 * d
y = 3 * d * e
```

The circuit is expected to run at a frequency of 20 MHz such that the entire computation (i.e., of both the expressions) takes no more than 250 ns, with the minimum possible cost.

The functional units multiplier and adder are available. The respective areas are 90 µm² and 30 µm², and the combinational delays are 90 ns and 30 ns respectively. Each multiplexer in the datapath has an area of 20 µm² and a delay of 20 ns, irrespective of the number of inputs. The datapath registers and the control unit do not contribute much to the area or timing.

Inputs a, b, c, d, and e as well as the constants (if required) are available in registers, and the results x, and y have to be stored in registers.

The cost of the circuit is \_\_\_\_\_\_ µm²

***

**Sol**. This might be the trickiest question in quiz 1, but if you've done the problem set 1 properly, this won't be very hard for you. To solve it systemetically, we can use the following steps

{% stepper %}
{% step %}
**Draw the sequencing graph**

Before drawing the sequencing graph, we should observe the expression and see if there are any parts that we can use shift (no cost) and addition to replace the multiplication symbol. In this case, we can replace `3d` by `d>>2+d`. Thus, our sequencing graph will look like below,

<figure><img src="../.gitbook/assets/quiz1-q9-sequencing-graph.svg" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
**Draw the scheduling table**

Before drawing the scheduling table, we should observe the constraints in this question:

1. The clock period is 50ns.
2. The latency of the circuit should be less than 5 clock cycles.
3. Registers have no cost.

Given these two constraints, we should be able to observe that:

1. It is impossible to share multipliers as the sharing of multipliers will add multiplexer to the inputs of the multiplier, thus make each multiplication take 3 cycles to complete. Also, as we share the multiplier, meaning that we have at least 2 multiplication operations happening sequentially, making it take 6 cycles in total to complete. This violates our latency constraint!
2. It is impossible to add multiplexer at the output of the multiplier. As the registers have no cost, there is no need to do register binding/sharing.
3. It is possible to share the ALU because the delay of one ALU + multiplexer is 30+20=50ns, which can be finished safely in one clock cycle.

With all this insights, we can draw our scheduled table as follows:

| t | Mult1 | Mult2 | Mult3 | ALU1 |
| - | ----- | ----- | ----- | ---- |
| 1 | 1     | —     | —     | 2    |
| 2 | 1     | 4     | —     | —    |
| 3 | —     | 4     | 3     | —    |
| 4 | —     | —     | 3     | —    |
| 5 | —     | —     | —     | 5    |

Based on the scheduled table, we can do the register binding. For the sake of simplicity, I did the register binding by labeling the sequencing graph instead of drawing arrows in between the table.

<figure><img src="../.gitbook/assets/quiz1-q9-register-bound-sequencing-graph.svg" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
**Datapath Synthesis**

After getting the scheduled table after doing the register binding, the datapath synthesis is very easy. And the datapath should look like as follows:

<figure><img src="../.gitbook/assets/quiz1-q9-datapath-synthesis.svg" alt=""><figcaption></figcaption></figure>

After doing the datapath synthesis, it is easier for us to calculate the cost of this circuit, which is

$$
3\times90+30+2\times20=340
$$
{% endstep %}

{% step %}
**Refine the Sharing**

Resource sharing is only reasonable if the two multiplexers cost less than another new resource. In this case, we might find out that the cost of 2 multiplexers are $$2\times20=40$$, which is **bigger** than another ALU! Thus, we shouldn't share the ALU! The refined scheduled table is shown as follows:

| t | Mult1 | Mult2 | Mult3 | ALU1 | ALU2 |
| - | ----- | ----- | ----- | ---- | ---- |
| 1 | 1     | —     | —     | 2    | —    |
| 2 | 1     | 4     | —     | —    | —    |
| 3 | —     | 4     | 3     | —    | —    |
| 4 | —     | —     | 3     | —    | —    |
| 5 | —     | —     | —     | —    | 5    |

And the refined datapath is shown below:

<figure><img src="../.gitbook/assets/quiz1-q9-datapath-synthesis-refined.svg" alt=""><figcaption></figcaption></figure>

The total cost is thus:

$$
3\times90+2\times30=330
$$
{% endstep %}
{% endstepper %}
