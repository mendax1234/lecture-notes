# Lec 2a - Pipelining

> This section and the next focuses on **RTL transformation**. The motivation is simple: if someone asks for a **2× improvement in throughput**, how should we modify the RTL to achieve it?
>
> As Prof. Massimo pointed out, this topic is often treated with heavy mathematical analysis in research. However, in this course, the goal is to build a sense of **intuition** instead. Rather than relying on purely equations, we focus on **visuals —** understanding and improving RTL structures by just inspecting them.

Pipelining involves inserting registers into a combinational block to divide it into $$n$$ stages. This allows different combinational parts to operate on different data in parallel ([parallelism in **temporal form**](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-06-advanced-processor#parallelism-in-temporal-and-spatial-form)), significantly improving **throughput**.

{% hint style="success" %}
We have seen pipelining in [NUS CG3207](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-05-the-pipelined-processor)!
{% endhint %}

The core idea of pipelining is:

> to reduce the clock cycle time ($$T_{\text{CK}}$$), we reduce the combinational delay between registers by splitting the logic, which is equivalent to reducing the **critical path.**

* **Throughput**: Increases because multiple stages process data simultaneously (parallelism).
* **Latency (clock cycles)**: Worsens (increases) due to the added delay of the inserted registers.

{% hint style="warning" %}
You will see a more detailed version of the throughput and latency change in the [#performance-analysis](lec-2a-pipelining.md#performance-analysis "mention").
{% endhint %}

<figure><img src="../.gitbook/assets/pipeline-example (1).png" alt=""><figcaption></figcaption></figure>

## Performance Analysis

We assume a perfectly balanced logic design where the total combinational delay is split evenly among stages, such that $$\tau_{\text{COMB,i}} = \frac{\tau_{\text{COMB}}}{n}$$.

{% hint style="warning" %}
Treat $$\tau_{\text{COMB}}$$ as a constant and $$n$$ is sometimes called the **pipeline depth**. We will prove at the [end](lec-2a-pipelining.md#logic-imbalance) of this section that the assumption we made above on the balanced combinational logic **doesn't matter** to our analysis!
{% endhint %}

### Throughput

> **Throughput** is defined as the number of computations completed per unit of time.

The minimum clock cycle time ($$T_{\text{pipe}}$$) is determined by the delay of a single stage plus the timing overhead ($$t_{\text{OH}}$$), which includes setup time and clock-to-Q delay:

$$
T_{\text{pipe}} = \max_{i}(\tau_{\text{COMB,i}} + t_{\text{OH,i}}) = \frac{\tau_{\text{COMB}}}{n} + t_{\text{OH}}
$$

$$n$$ is the pipeline depth. $$t_{\text{OH}}$$ is the overhead (setup time, clock-to-Q delay).

{% hint style="warning" %}
The $$T_{\text{pipe}}$$ here is equal to $$T_{\text{CK}}$$ and we assume that we have the **same pipeline register overhead**, which means that **same type** of register is used and we are not adding any [skew](lec-1b-timing-synchronous.md#clock-skew) to the clock of the register.
{% endhint %}

#### Throughput Improvement

The ratio of the new frequency ($$f_{\text{pipe}}$$) to the original frequency ($$f$$) is:

$$
\frac{f_{\text{pipe}}}{f} = n \frac{\tau_{COMB} + t_{OH}}{\tau_{COMB} + n \cdot t_{OH}}
$$

<figure><img src="../.gitbook/assets/pipeline-throughput-frequency-improvement (1).png" alt="" width="563"><figcaption></figcaption></figure>

* Case 1 ($$n \ll \frac{\tau_{\text{COMB}}}{t_{\text{OH}}}$$): Improvement is linear (factor of $$n$$). To verify, can neglect $$n\cdot t_{\text{OH}}$$.
* Case 2 ($$n > \frac{\tau_{\text{COMB}}}{t_{\text{OH}}}$$): Improvement saturates to a maximum of $$1 + \frac{\tau_{\text{COMB}}}{t_{\text{OH}}}$$. To verify, can neglect the term $$\tau_{\text{COMB}}$$ in the denominator. So as $$n\to +\infty$$, the ratio $$\to 1+\frac{\tau_{\text{COMB}}}{t_{\text{OH}}}$$.

The intuition behind this formula is that beyond a certain point, increasing the number of pipeline stages ($$n$$) — e.g., making the pipeline too deep — yields diminishing returns. In other words, the throughput improvement eventually **saturates** rather than continuing to scale.

This also foreshadows ideas in later chapters: **retiming** mainly works by adding or relocating registers, but adding registers indiscriminately does not always improve performance. There is a fundamental limit, which we will understand more clearly later.

<details>

<summary>How Throughput relates to Frequency here?</summary>

In Lec 1b, we have learned that throughput is

> **Throughput** is defined as the number of computations completed per unit of time.

<p align="center"><span class="math">\text{Throughput} = \frac{\text{Number of Computations}}{\text{Time (seconds)}}</span></p>

Rewrite the equation,

<p align="center"><span class="math">\text{Throughput} = \frac{\text{Computations}}{\text{Second}} = \frac{\text{Computations}}{\text{Cycle}} \times \frac{\text{Cycles}}{\text{Second}}</span></p>

As a pipelined processor completes one instruction (or one "computation") every single clock cycle after the pipeline overhead. Also the definition of clock frequency is

> Clock frequency is the **speed** at which a computer's processor executes internal operations, measured in **Hertz** (Hz), usually gigahertz (GHz) for modern CPUs, representing billions of "ticks" or **cycles per second.**

Thus, we can write the throughput formula as follows:

<p align="center"><span class="math">\text{Throughput} = 1 \times \text{Frequency} = f</span></p>

Therefore, when we say we are "improving throughput" in pipelining, we are doing so by increasing the clock frequency ($$f_{\text{pipe}}$$). The faster the clock ticks, the more computations finish per second.

</details>

### Latency

> **Latency** is the _time/clock cycles_ required to complete a **single** computation from the moment inputs arrive until the final output is valid.

If we divide our system into $$n$$ stages, then one operation will take $$n$$ stages to complete, thus,

$$
\text{LAT}_{\text{pipe}} = n \cdot T_{\text{pipe}} = \tau_{COMB} + n \cdot t_{OH}
$$

#### Latency Improvement

The ratio of the new latency measured in **absolute time** ($$\text{LAT}_{\text{pipe}}$$) to the original latency($$\text{LAT}=\tau_{COMB}+t_{OH}$$) is:

$$
\frac{\text{LAT}_{\text{pipe}}}{\text{LAT}} = \frac{n \cdot T_{\text{pipe}}}{T} = \frac{\tau_{\text{COMB}} + n \cdot t_{\text{OH}}}{\tau_{\text{COMB}} + t_{\text{OH}}}
$$

After simplifying it, we can get,

$$
\begin{aligned}
\frac{\text{LAT}_{\text{pipe}}}{\text{LAT}} &= \frac{1 + n \cdot \frac{t_{\text{OH}}}{\tau_{\text{COMB}}}}{1 + \frac{t_{\text{OH}}}{\tau_{\text{COMB}}}} \\
&\approx \left( 1 + n \cdot \frac{t_{\text{OH}}}{\tau_{\text{COMB}}} \right) \cdot \left( 1 - \frac{t_{\text{OH}}}{\tau_{\text{COMB}}} \right)
\end{aligned}
$$

{% hint style="success" %}
To do the simplification, we first divide both the nominator and the denominator with $$\tau_{\text{COMB}}$$. We then apply the geometric series approximation $$\frac{1}{1+x} \approx 1-x$$, which is valid when $$x \ll 1$$. In this context, we assume the timing overhead is small relative to the combinational delay ($$t_{\text{OH}} \ll \tau_{\text{COMB}}$$).
{% endhint %}

Expanding the product yields $$1 + (n-1)\frac{t_{\text{OH}}}{\tau_{\text{COMB}}} - n(\frac{t_{\text{OH}}}{\tau_{\text{COMB}}})^2$$. By neglecting the second-order term (the squared component), we arrive at the final linear approximation:

$$
\frac{\text{LAT}_{\text{pipe}}}{\text{LAT}} \approx 1 + (n-1)\frac{t_{\text{OH}}}{\tau_{\text{COMB}}}
$$

From this equation, we can only tell that in terms of **time**, latency increases linearly with the pipeline stages $$n$$.

Using the method we've seen in Lec 01b, in terms of **clock cycles**, latency increases by $$(n-1)$$ because we add $$n-1$$ registers to our design.

## Power & Area

### Area

The total silicon area is calculated as follow:

$$
A=\sum_{i=1}^{n}A_{\text{COMB,i}}+\sum_{i=1}^{n+1}A_{\text{reg,i}}
$$

We assume area is gate-dominated ($$\sum_{i=1}^{n}A_{\text{COMB,i}}=A_{\text{COMB}}$$) and the register area ($$A_{\text{reg,i}}$$) is small ( $$A_{\text{reg,i}} \ll A_{\text{COMB}}$$).

$$
A_{\text{pipe}} = \sum A_{\text{COMB,i}} + \sum_{i=1}^{n+1} A_{\text{reg,i}} = A_{\text{COMB}} + (n+1)A_{\text{reg,i}}
$$

{% hint style="success" %}
Assume that the area for all registers used is the same and it is $$A_{\text{reg,i}}$$.
{% endhint %}

#### Area Improvement

After that, the ratio of the pipelined version's area over the normal version's area will be as follow:

$$
\frac{A_{\text{pipe}}}{A} = \frac{A_{\text{COMB}} + (n+1)A_{\text{reg,i}}}{A_{\text{COMB}} + 2A_{\text{reg,i}}} \approx 1 + (n-1)\frac{A_{\text{reg,i}}}{A_{\text{COMB}}}
$$

{% hint style="success" %}
The same techinique used in the [#latency-improvement](lec-2a-pipelining.md#latency-improvement "mention"), which is to divide both the nominators and denominators with $$A_{\text{COMB}}$$ and then use the geometric series approximation. $$\frac{1}{1+x} \approx 1-x$$ is used here to get the final approximation.
{% endhint %}

Area overhead grows **linearly** as we increase the number of stages ($$n$$).

### Power

> Power can be also replaced with **energy**.

The total energy in a system is calculated as follow:

$$
E=\sum_{i=1}^{n}E_{\text{COMB,i}}+\sum_{i=1}^{n+1}E_{\text{reg,i}}
$$

#### Energy Improvement

We assume the same switching activity/glitching ($$\sum_{i=1}^{n}E_{\text{COMB,i}}=E_{\text{COMB}}$$) and the $$E_{\text{reg,i}} \ll E_{\text{COMB}}$$). We can use the similar or almost the same steps from the [area analysis](lec-2a-pipelining.md#silicon-area) above to get the following formula:

$$
\frac{E_{\text{pipe}}}{E} = \frac{E_{\text{COMB}} + (n+1)E_{\text{reg,i}}}{E_{\text{COMB}} + 2E_{\text{reg,i}}} \approx 1 + (n-1)\frac{E_{\text{reg,i}}}{E_{\text{COMB}}}
$$

Similar to the [area](lec-2a-pipelining.md#silicon-area), the energy grows **linearly** with the number of stage ($$n$$).

#### PPA Analysis Summary

| Metric               | Sequential (baseline) | n-stage Pipelined Design              |
| -------------------- | --------------------- | ------------------------------------- |
| Throughput           | 1                     | ≈ n                                   |
| Latency (Time)       | 1                     | > 1 (slight increase due to overhead) |
| Latency (Cycles)     | 1                     | n                                     |
| Clock Frequency      | f                     | ≈ n · f                               |
| Area                 | 1                     | > 1 (linear growth due to registers)  |
| Energy per Operation | 1                     | > 1 (linear growth due to registers)  |

## Pipeline Operations & Stalls

### Filling the pipeline

The pipeline has two states:

1. **Transient (Filling)**: The pipeline is not yet full. It fills up stage-by-stage.
2. **Steady State**: Occurs when the last stage ($$n$$-th) activates. From this point on, one output is generated every clock period.

<figure><img src="../.gitbook/assets/data-flow-in-pipeline (1).png" alt=""><figcaption></figcaption></figure>

### Stalls

If the pipeline is not always filled, performance drops. A "stall" means a stage is not computing useful data during a cycle.

#### Throughput with Stalls

We assume that for each operation, there are $$\delta$$ stalled cycles on average. So, one computation is performed every $$T_{\text{pipe}}(1+\delta)$$ instead of $$T_{\text{pipe}}$$. Thus the throughput will result to,

$$
\text{Throughput} = \frac{1}{T_{\text{pipe}}(1+\delta)}
$$

Stalls effectively increase the CPI (in the [NUS CG3207 terminology](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-01-history-technology-performance#instruction-count-ic-and-cpi)), thus reducing throughput.

{% hint style="success" %}
This $$\delta$$ also explains why, in an ideal pipeline design, **throughput equals frequency**. In this case, the stall $$\delta$$ is zero, so the throughput is

<p align="center"><span class="math">\text{Throughput} = \frac{1}{T_{\text{pipe}}} = \text{frequency}</span></p>
{% endhint %}

## Logic Imbalance

In practical designs, it is rarely possible to partition logic into perfectly balanced pipeline stages. This is because delays are inherently **discrete**, not continuous. Although retiming (will see later) allows us to redistribute combinational delay across stages, the discrete nature of logic elements makes it difficult to achieve equal delay in every part of the system.

As the pipeline speed is always limited by the bottleneck (the slowest stage), we define the imbalance of a stage $$i$$ as the difference between its actual delay and the ideal average delay:

$$
\Delta \tau_{\text{COMB,i}} = \tau_{\text{COMB,i}} - \frac{\tau_{\text{COMB}}}{n}
$$

The clock cycle ($$T_{\text{CK}}$$) must be long enough to cover the delay of the slowest stage plus register overheads ($$\tau_{\text{OH,REG}} + \tau_{\text{OH,clocking}}$$).

$$
T_{CK} \ge \max_{i}(\tau_{\text{COMB,i}}) + \tau_{\text{OH,REG}} + \tau_{\text{OH,clocking}}
$$

By substituting the definition of imbalance ($$\tau_{\text{COMB,i}} = \frac{\tau_{\text{COMB}}}{n} + \Delta \tau_{\text{COMB,i}}$$), we can rewrite the equation:

$$
T_{\text{CK}} \ge \left( \frac{\tau_{\text{COMB}}}{n} + \max_{i}(\Delta \tau_{\text{COMB,i}}) \right) + \tau_{\text{OH,REG}} + \tau_{\text{OH,clocking}}
$$

We can group the **imbalance term** with the hardware overheads to define a new effective overhead ($$t_{\text{OH}}$$):

$$
t_{\text{OH}} = \underbrace{\max_{i}(\Delta \tau_{\text{COMB,i}})}_{\text{Imbalance}} + \underbrace{\tau_{\text{OH,REG}} + \tau_{\text{OH,clocking}}}_{\text{Hardware Costs}}
$$

Resulting in the simplified pipeline equation:

$$
T_{CK} = \frac{\tau_{\text{COMB}}}{n} + t_{\text{OH}}
$$

In conclusion, maximum imbalance behaves mathematically identical to hardware **overhead**. It acts as a constant penalty that prevents the pipeline from achieving the **ideal linear speedup**.

{% hint style="warning" %}
Practice this in the upcoming labs!
{% endhint %}
