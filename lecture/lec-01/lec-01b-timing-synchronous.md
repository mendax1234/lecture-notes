# Lec 01b - Timing Synchronous

## Synchronous VLSI Systems

A digital system consists of communicating blocks shown as follows,

<figure><img src="../../.gitbook/assets/communication-blocks.png" alt="" width="563"><figcaption></figcaption></figure>

Each combinational module evaluates its output in

$$
\tau_{\text{comb}} \in [\tau_{\text{comb},\text{min}},\tau_{\text{comb},\text{max}}]\;(\text{input-dependent})
$$

For example, the module 2 needs to "know" when in<sub>2</sub> / out<sub>1</sub> is correct (has settled). This can be illustrated using the following figure,

<figure><img src="../../.gitbook/assets/communication-between-digital-modules.png" alt="" width="563"><figcaption></figcaption></figure>

In module 1, the <mark style="color:green;">fast path</mark> gives earliest possible correct output ($$\tau_{\text{comb},\text{min}}$$), while the <mark style="color:red;">slow path</mark> gives the latest settling output ($$\tau_{\text{comb},\text{max}}$$). The correct out<sub>1</sub> settles between $$\tau_{\text{comb},\text{min}}$$ and $$\tau_{\text{comb},\text{max}}$$. So, to ensure out<sub>1</sub> is correct, we wait until after $$\tau_{\text{comb},\text{max}}$$. The again highlight the importance of [**critical path**](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#critical-path)!

{% hint style="info" %}
#### Some Notations

1. **Cross-hatched (zig-zag) region**: This means that the signal is **unstable**. So, in<sub>1</sub> is stable only after the first dashed line.
2. **Flat solid line:** This means that the signal is **stable**.&#x20;

These two notations have appeared in [Harris and Harris's DDCA](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#propagation-and-contamination-delay)! Thus we can also say that

1. $$\tau_{\text{comb},\text{min}}$$ is called the **contamination delay**.
2. $$\tau_{\text{comb},\text{max}}$$ is called the **propagation delay**.
{% endhint %}

### Synchronous vs. Asynchronous

There are two approaches to ensure correct inputs:

* Synchronous
* Asynchronous

#### Asynchronous

In asynchronous mode, additional circuitry generates "completion signal" and this completion signal enables computation of next block when its output is valid.

<figure><img src="../../.gitbook/assets/asynchronous-communication-mode.png" alt="" width="563"><figcaption></figcaption></figure>

#### Synchrnous

In synchronous mode, all I/O signals are synchronized to clock clk. The clock periodically triggers next computation. All I/O signals are only allowed to be _used_ at **clock events**. And all the computation is done during the clock period.

{% stepper %}
{% step %}
#### Clock Event

At the clock event:

* Registers sample their inputs
* Registers update their outputs
* The _next computation cycle begins_

{% hint style="info" %}
Think of the above steps as: “Everyone stop, look at your inputs, remember them, then start computing again.”
{% endhint %}
{% endstep %}

{% step %}
#### Clock Period

During the clock period:

* Registers hold stable values at their outputs
* Combinational logic computes new values
* Results must be ready **before the next clock event**. If not, will cause timing violation!
{% endstep %}
{% endstepper %}

In the timing diagram, the first time shift represents the **Setup Time** (t<sub>setup</sub>), which is the required window where input signal `x` must be stable before the clock edge to be sampled correctly. The second time shift represents the **Clock-to-Output Delay** (t<sub>co</sub>), which is the time it takes for the register to react to the clock edge and update the output signal `y` before it propagates to the subsequent combinational logic.

<figure><img src="../../.gitbook/assets/synchronous-communication-mode.png" alt="" width="563"><figcaption></figcaption></figure>

And if we draw a register diagram, it will be similar to the following

<figure><picture><source srcset="../../.gitbook/assets/lec01-synchronous-example-dark.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/lec01-synchronous-example-light.png" alt=""></picture><figcaption></figcaption></figure>

{% hint style="danger" %}
In both cases of synchronous and asynchronous design, energy/timing/area overhead is paid for.
{% endhint %}

## Sequencing in Synchronous Systems

In digital logic, the order of data flow is critical. Usually, we want to achieve the **Deterministic Sequencing**. We require a strict First-In, First-Out (FIFO) behavior, where the n<sup>th</sup> input produces the n<sup>th</sup> output in the exact same sequence. Ideally: This is easy to achieve if the delay through a module is data independent (e.g., every calculation takes the exact same amount of time).

<figure><img src="../../.gitbook/assets/lec01-sequence-synchronous-data-independent.png" alt=""><figcaption></figcaption></figure>

However, in reality, In real silicon, the time it takes for a signal to propagate through a combinational block depends on the specific input values. As we have seen in [Harris & Harris DDCA](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#propagation-and-contamination-delay):

* Propagation Delay (t<sub>pd</sub>): The time taken by the _slowest_ path (critical path).
* Contamination Delay (t<sub>cd</sub>): The time taken by the _fastest_ path (short path).

<figure><img src="../../.gitbook/assets/lec01-sequence-synchronous-data-dependent.png" alt=""><figcaption></figcaption></figure>

If we connect modules directly without synchronization (asynchronous design), "Fast" data can overtake "Slow" data. For example,

1. **Input A (Complex):** We send a Division instruction (Slow Path). It begins processing.
2. **Input B (Simple)**: Immediately after, we send an Add instruction (Fast Path).

Because the Adder logic is much faster (shorter contamination delay) than the Divider logic, the result of the **Add** races through the circuit and appears at the output _before_ the Division is finished. So, the system reads the wrong result order (`Result B` -> `Result A`). This is a **Race Condition**, leading to data corruption and system failure.

To prevent the "race conditions" described previously, we enforce a strict rule: Data must move by exactly one stage per clock cycle. This is also called the **lockedstep movement**.

* This movement is controlled by a global **Clock Event** (usually the rising or falling edge of a signal).
* In a synchronous system, we ignore the transient "noise" or glitches that happen between clock edges.
  * We define the signal's value at the i-th cycle as the _single_ stable value present right before the sampling edge.
  * Any changes happening _between_ cycle i and i+1 are considered "work in progress" and are ignored until the next clock edge arrives.

<figure><img src="../../.gitbook/assets/lec01-sequencing-with-register.png" alt=""><figcaption></figcaption></figure>

The abstract "Box" shown in the sequencing diagrams is implemented using storage elements. As recalled from EE2026 and [_Harris & Harris DDCA_](https://wenbo-notes.gitbook.io/ddca-notes/textbook/sequential-logic-design/latches-and-flip-flops)_**.**_
