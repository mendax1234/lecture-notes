# Quiz 3

## Problems

### 1. Placement for high performance

**Question**: For analytical placers, using a quadratic cost function is better-suited for high-performance circuits as compared to using a linear cost function.

* [x] True
* [ ] False

***

**Sol.** The answer is **true**. We've seen the implications of linear vs. quadratic cost function in the [lecture](https://wenbo-notes.gitbook.io/ee4218-notes/lec/lec-09-physical-synthesis#implications-of-different-cost-functions), which is

> 1. The **quadratic** cost function tends to minimize the **standard deviation** of wires, which will penalize long wires and might give us better timing performance. But the average wire length might go up compared to the linear cost function.
> 2. The **linear** cost function tends to minimize the **total wire length**, which tends to minimize the cost.

Thus, in this question, for better high performance/timing performance, we should use **quadratic cost function**.

### 2. Routing

**Question**: It is better to use multiple shorter wires instead of longer wires on the critical path.

* [ ] True
* [x] False

***

**Sol**. This is a tricky question and the answer is **false**. Because in FPGAs, having multiple short wires means that we will need to have more switches to connect these multiple short wires. This overhead will increase the timing for the critical path!

{% hint style="danger" %}
More flexibility means slower performance! In this case, although multiple short wires can give us more flexibility, but it may affect negatively on our critical path.
{% endhint %}

### 3. Placement -> Routing

**Question**: Finding an optimal solution to the placement problem is not sufficient to guarantee routability in an FPGA.

* [x] True
* [ ] False

***

**Sol**. The answer is obviously **true**. Placement only guarantees that everything fits into the silicon canva from a function perspective but no guarantee that we can connect it!

### 4. FPGA Switch Box

**Question**: FPGA switchboxes generally have switches for all possible directions.

* [ ] True
* [x] False

***

**Sol.** This is obviously **false**. The truth is that the switch box only has a **subset** of all the possible connections instead of allowing everything.

### 5. Routing Algorithm

**Question**: Splitting the routing step into two sub-steps: global routing and detailed routing, helps increase the optimality of routing.

* [ ] True
* [x] False

***

**Sol**. The answer is obviously **false**. We've seen this in the [lecture notes](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/part-1-lec-digital-design-flow/lec-3a-digital-design-flow#routing)!

### 6. HLS Pipeline

**Question**: Pipelining generally results in a reduction in the latency per iteration as compared to a multi-cycle design.

* [ ] True
* [x] False

***

**Sol**. The answer is obviously **false**. One example has appeared in our [lecture](../lec/lec-06-high-level-synthesis.md#pipelined) before! Pipeline doesn't help with the latency per iteration! It helps improve the **throughput** via the improvement of the initiation interval.

### 7. HLS Optimization Pragmas

**Question**: Which of the following HLS directives generally results in a reduction in the cost of the hardware?

* [ ] PIPELINE
* [ ] UNROLL
* [ ] ARRAY\_PARTITION
* [x] None of the above

***

**Sol**. The answer is obviously **none of the above**. As for all the HLS pragmas we have encountered in this class, they are all trading the area for better performance.

{% hint style="warning" %}
There are some pragmas that can reduce the hardware!
{% endhint %}

### 8. HLS loop unrolling

**Question**: HLS tools are generally able to generate more optimized hardware if the various loops in the function have data-independent exit conditions.

* [x] True
* [ ] False

***

**Sol**. The answer is obviously **true**.
