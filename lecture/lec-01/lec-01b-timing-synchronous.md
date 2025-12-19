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

These two notations have appeared in [Harris and Harris's DDCA](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#propagation-and-contamination-delay)!
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

> The following diagram, not too sure how it works. Wait for Prof Massimo's explanation!

<figure><img src="../../.gitbook/assets/synchronous-communication-mode.png" alt="" width="563"><figcaption></figcaption></figure>
