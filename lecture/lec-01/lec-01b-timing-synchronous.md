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

However, in reality, the time it takes for a signal to propagate through a combinational block depends on the specific input values. As we have seen in [Harris & Harris DDCA](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#propagation-and-contamination-delay):

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

## Clock Non Idealities

Ideally, the clock signal is

1. instantaneously distributed to all flip-flops (FFs)
2. periodic

However, as clock is distributed throughout the chip with wires + repeaters:

1. delays in clock distribution network maybe different
2. different **arrival time t**<sub>**i**</sub> of clock signal clk<sub>i</sub> (same clcok signal but arrives at different FFs at different time)

<details>

<summary>Repeater and Interconnect Wire</summary>

<figure><img src="../../.gitbook/assets/repeater-interconnect-wire.png" alt=""><figcaption></figcaption></figure>

First, let's look at the symbols in the diagram above,

* **The Repeater (Triangle)**: This is a buffer or amplifier. Clock signals have to travel long distances across a chip. Repeaters are placed at intervals to "boost" the signal back up so it stays strong.
* **The Interconnect Wire (The shaded bar)**: This is the physical metal wire connecting the clock source to the Flip-Flops (FF<sub>i</sub> and FF<sub>j</sub>).
* **The "Squiggly" and "Parallel lines" below the wire**: This is an electrical circuit model (**RC Model**) of the wire.
  * Real wires are not perfect conductors. They have **Resistance**. And real wires interact with the ground/substrate. They have **Capacitance**.
  * **The Result**: To get the voltage from the Repeater to the Flip-Flop, the current has to "fill up" those capacitors through those resistors. This takes physical time (called RC Delay).

The wire leading to FF<sub>i</sub> might be slightly longer, or have slightly different resistance/capacitance than the wire leading to FF<sub>j</sub>. Therefore, it takes a different amount of time for the clock signal to charge up the wire and trigger FF<sub>i</sub> compared to FF<sub>j</sub>.

{% hint style="info" %}
In digital circuits, all information — including the clock signal — travels in the form of **Voltage**.
{% endhint %}

</details>

### Clock Skew

We define the DC time shift of clk<sub>i</sub> w.r.t. clk<sub>j</sub>, which is also the clock skew seen by FF<sub>i</sub> w.r.t. FF<sub>j</sub>, to be

$$
t_{\text{skew, ij}}=t_i-t_j
$$

{% hint style="info" %}
In synchronous circuit design, while we originate from a **single global clock source** (CLK),

* clk<sub>i</sub> refers to the local clock signal branch connected to FF<sub>i</sub>.&#x20;
* The variable t<sub>i</sub> denotes the specific time instant when the clock edge actually arrives at FF<sub>i</sub>.
{% endhint %}

When distributing clk in the **same direction** as data flow, the skew t<sub>skew, ij</sub> is positive. If **opposite direction**, the skew is **nagative**. For example, the following diagram shows a **positive clock skew** by assuming that the data and clock distribution flows from register R1 to register R2.

<figure><img src="../../.gitbook/assets/clock-skew-sign-example.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
An analogy is to think of the clock skew as a **vector** and the two points are t<sub>i</sub> and t<sub>j</sub>. Thus,

<p align="center"><span class="math">t_{\text{skew, ij}}=t_i-t_j=-t_{\text{skew, ji}}</span></p>
{% endhint %}

Another example will be the timing diagram we have seen in [Harris & Harris DDCA](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/sequential-logic-design/timing-of-sequential-logic#clock-skew), the following diagram will indicate a **negative skew** seen by R1 (t<sub>1</sub> - t<sub>2</sub> < 0, assuming the data flows from R1 to R2).

<figure><img src="../../.gitbook/assets/clock-skew-example-ddca.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
The clock skew is relative. But we usually calculate the clock skew following the direction of the data path.

* If data flows from register A to register B, we calculate skew as t<sub>B</sub>- t<sub>A</sub>. During the calculation, treat t<sub>B</sub> and t<sub>A</sub> as two **algebric values** and the result will be either negative or positive!
{% endhint %}

#### Clock Skew Calculation

The clock skew can randomly vary due to delay variations, but it can be calculated using the following formula,

$$
t_{\text{skew}} = t_{\text{skew,DET}} \pm \left| t_{\text{skew,RAND}} \right|
$$

From this formula, we can see that the clock skew has two components

1. **Deterministic skew**: Predictable delay caused by the fixed physical layout of wires and repeaters. Since its sign and magnitude are known, it can be intentionally engineered to optimize timing paths (useful skew).
2. **Random skew**: Unpredictable variation arising from manufacturing mismatches or environmental factors like temperature. It creates a bounded uncertainty range ($$\pm$$) with an unknown sign that designers must account for as noise.

### Clock Jitter

In reality, the clock period T<sub>CK</sub> is not perfectly constant due to jitter. Specifically, we define **cycle-to-cycle jitter** (t<sub>jitter</sub>) as the random, time-varying deviation between two successive clock events (such as two rising clock edges). This means the actual clock period is not fixed; instead, it fluctuates around a nominal period (T<sub>nom</sub>), strictly bounded within the range of $$T_{\text{nom}} - |t_{\text{jitter}}|$$ to $$T_{\text{nom}} + |t_{\text{jitter}}|$$.

<figure><img src="../../.gitbook/assets/clock-jitter.png" alt=""><figcaption></figcaption></figure>

#### The Reason of Clock Jitter

The two main reasons for clock jitter are:

1. **clock generator**'s intrinsic jitter
2. **clock distribution network**: due to time-varying delay of repeaters (supply noise)

<figure><img src="../../.gitbook/assets/clock-jitter-cause.png" alt=""><figcaption></figcaption></figure>

Based on the above diagram, we have the formula for calculating the t<sub>jitter,i</sub> to be

$$
t_{\text{jitter},i} = t_{\text{jitter,clock\_gen}} + \sum_{j=1}^{n} \left| \Delta \tau_{\text{PD,buffer},j} \right|
$$

## Timing Parameters of Edge-Triggered Flip Flops

> This section has a lot of similarities with the [Timing of Sequential circuits](https://app.gitbook.com/o/MnEKr5A4lYXtOfhoXGj5/s/jTJFBPtKk6NwweAooH53/) in Harris & Harris DDCA!

In positive-edge triggered (PET) flip flops, input is **sampled** at rising clock edge. And the **timing parameters** for D Flip Flops are:

1. input must be kept stable from **t**<sub>**SETUP**</sub>**&#x20;before** the active edge to **t**<sub>**HOLD**</sub>**&#x20;after** this edge. Otherwise, we will have **metastability**.
2. CK-Q delay: output is updated at t<sub>CK-Q</sub> after clock edge.

<figure><img src="../../.gitbook/assets/timing-parameters-for-edge-triggered-ffs.png" alt="" width="563"><figcaption></figcaption></figure>

### Asynchronous Resettable Filp Flops

> This section is partly discussed in the [resettable flip flops](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/sequential-logic-design/latches-and-flip-flops#resettable-flip-flop) in Harris & Harris DDCA!

In asynchronous resettable flip flops, the RESET signal has **higher** priority than the CLK signal. So sometimes the RESET can be used to "mask" (hide or block) the CLK signal causing the fact that the Flip-Flop didn't "see" the clock rise because the RESET was blinding it.

Simiarly, its timing parameters are:

1. For normal clock event, reset must be kept stable from **t**<sub>**RECOVERY**</sub>**&#x20;before** clock edge to **t**<sub>**REMOVAL**</sub>**&#x20;after** this edge.

For example, the following diagram shows an active-low asynchronous reset

<figure><img src="../../.gitbook/assets/active-low-asynchronous-reset-ffs.png" alt=""><figcaption></figcaption></figure>

* If you want to enable the clock event, release the RESET button at least t<sub>RECOVERY</sub> before the rising clock edge.
* If you want to ignore the clock event, press and hold the RESET and don't release until t<sub>REMOVAL</sub> after the rising clock edge.

{% hint style="warning" %}
Even if this is an active-low asynchronous resettable FF, pressing the RESET button will reset the FF, meaning that pressing the RESET button is equivalent to set RESET signal to be 0.
{% endhint %}

## Timing Constraints in Synchronous Circuits

> Again, this part is almost the same as [Timing of Sequential Circuits](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/sequential-logic-design/timing-of-sequential-logic) covered in Harris & Harris DDCA. But with some more info added on the **timing overhead** caused by clock skew and clock jitter.

The FF timing constraints imply the system timing constraints. The FF timing constraints have

* Setup Time Constraint
* Hold Time Constraint

While the system timing constraint is more about the global CLK signal speed. So, what the first sentence says is that the FF timing constraint will affect the speed of the system clock, which is an indispensible part of the system timing constraint. So, when designing a system, we should prevent setup/hold violations.

System timing constraints are affected by

* FF time constraints
* combinational logic timing parameters
* clock non-idealities (skew and jitter)

To start, let's first see an intuitive understanding of FF timing constraints.

<figure><img src="../../.gitbook/assets/intuitive-understand-ff-timing-constraint.png" alt="" width="563"><figcaption></figcaption></figure>

* To meet the setup time constraint, we can think of it as "the computation should be completed before next edge in REG<sub>2</sub>" -> This gives us the **max-delay constraint** for the combinational logic
* To meet the hold time constraint, we can think of it as "the computation must affect REG<sub>2</sub> only after a certain time" -> This gives us the **min-delay constraint** for the combinational logic

#### Terminology Mapping

As the following parts are mostly covered in [DICADP](../../textbook-1-dicadp/timing-issues-in-digital-circuits.md#synchronous-design-an-in-depth-perspective), here is the table summarizing the difference between some terminologies used:

| DICADP (Textbook1)      | EE4415                                                           | Description                                                                                           |
| ----------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| $$T_{CLK}$$ or $$T$$    | $$T_{CK}$$                                                       | Nominal clock period                                                                                  |
| $$t_{logic}$$           | $$\tau_{\text{COMB,max}}$$                                       | Maximum propagation delay through combinational logic                                                 |
| $$t_{logic,cd}$$        | $$\tau_{\text{COMB,min}}$$                                       | Minimum contamination delay through combinational logic                                               |
| $$t_{c-q}$$             | $$\tau_{\text{CK-Q},\text{REG1}}$$                               | Register clock-to-Q propagation delay                                                                 |
| $$t_{c-q, cd}$$         | $$\tau_{\text{CK-Q},\text{REG1}}$$                               | Register clock-to-Q contamination delay (Don't know why EE4415 uses the same notation for both :joy:) |
| $$t_{su}$$              | $$t_{\text{SETUP},\text{REG2}}$$                                 | Setup time of the destination register                                                                |
| $$t_{hold}$$            | $$t_{\text{HOLD},\text{REG2}}$$                                  | Hold time of the destination register                                                                 |
| $$\delta$$ (Clock Skew) | $$t_{\text{skew,DET}} \pm \left|t_{\text{skew,RAND,21}}\right|$$ | Clock skew (deterministic ± random in EE4415 but only deterministic in DICADP)                        |
| $$t_{\text{jitter}}$$   | $$t_{\text{jitter}}$$                                            | Clock jitter                                                                                          |

{% hint style="danger" %}
In EE4415, the definition of clock jitter is a bit different from DICADP. So, we must follow EE4415's logic: **Jitter cancels out for Hold Time** because it is a "common mode" noise source in the same clock domain. However, in EE4415 we introduces Random Skew ($$|t_{skew,RAND}|$$) to account for the variation that _does_ differ between the two registers (like thermal noise in the wires), which the DICADP textbook might have just lumped into "jitter."
{% endhint %}

### Max-Delay Constraint

{% tabs %}
{% tab title="Textbook DICADP" %}
In the textbook, if we take both clock skew and clock jitter into account, the formula we get is

$$
T_{\text{CLK}} + \delta - t_{\text{jitter}1} - t_{\text{jitter}2} \geq t_{\text{c-q}} + t_{\text{logic}} + t_{\text{su}} 
\\ \text{or} \\ T \geq t_{\text{c-q}} + t_{\text{logic}} + t_{\text{su}} - \delta + t_{\text{jitter}1} + t_{\text{jitter}2} \tag{1}
$$

{% hint style="info" %}
The t<sub>jitter1</sub> and t<sub>jitter2</sub> can be treated as the same.
{% endhint %}
{% endtab %}

{% tab title="EE4415" %}
Use the table above to map the formula into EE4415, we will get

$$
T_{\text{CK}} \ge \tau_{\text{COMB,max}} + t_{\text{SETUP, REG2}} + \tau_{\text{CK-Q, REG1}} - t_{\text{skew,DET}} + |t_{\text{skew,RAND,21}}| + 2|t_{\text{jitter}}| \\ 
\text{or} \\ 
\tau_{\text{COMB,max}} \le T_{\text{CK}} - t_{\text{SETUP, REG2}} - \tau_{\text{CK-Q, REG1}} + t_{\text{skew,DET}} - |t_{\text{skew,RAND,21}}| - 2|t_{\text{jitter}}| \tag{2}
$$

{% hint style="warning" %}
As the spirit is to make R.H.S as big as possible (worst-case scenraio), we will map the clock skew $$\delta$$ to $$t_{\text{skew,DET}} - |t_{\text{skew,RAND}}|$$.
{% endhint %}

To simplify it, we group some terms and get the following

$$
\tau_{COMB,max} \le T_{CK} - t_{OH} \\ \text{where} \\ t_{\text{OH}}= t_{\text{SETUP}} + \tau_{\text{CK-Q}} - t_{\text{skew,DET}} + |t_{\text{skew,RAND}}| + 2|t_{\text{jitter}}|
$$

<figure><img src="../../.gitbook/assets/max-delay-constraint.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Min-Delay Constraint

{% tabs %}
{% tab title="Textbook DICADP" %}
Similarly, for the hold time constraint / min-delay constraint, the formula we get is,

$$
\delta + t_{\text{hold}} + t_{\text{jitter}1} + t_{\text{jitter}2} < t_{(\text{c-q, cd})} + t_{(\text{logic, cd})} \\
\text{or} \\
\delta < t_{(\text{c-q, cd})} + t_{(\text{logic, cd})} - t_{\text{hold}} - t_{\text{jitter}1} - t_{\text{jitter}2} \tag{3}
$$
{% endtab %}

{% tab title="EE4415" %}
To map the formula into EE4415, we will get,

$$
\tau_{\text{COMB,min}} + \tau_{\text{CK-Q, REG1}} \ge t_{\text{HOLD,REG2,eq}} \\ \text{where} \\ t_{\text{HOLD,REG2,eq}} = t_{\text{HOLD, REG2}} + t_{\text{skew,DET}} + |t_{\text{skew,RAND,21}}| \tag{4}
$$

Rearrange it, we will get,

$$
\tau_{\text{COMB,min}} \ge t_{\text{HOLD,REG2,eq}} - \tau_{\text{CK-Q,REG1}}
$$

<figure><img src="../../.gitbook/assets/min-delay-constraint.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

## Intentional Skew

As we have seen in DICADP, positive skew will improve **performance** but decrease hold robustness while negative skew will decrease performance but improve hold robustness.

From the Eq. (2) in [#min-delay-constraint](lec-01b-timing-synchronous.md#min-delay-constraint "mention"), we can derive the maximum positive skew as follows,

$$
t_{\text{skew,DET,max}}
=
\tau_{\text{COMB,min}}
+
\tau_{\text{CK-Q,REG1}}
-
t_{\text{HOLD,REG2}}
-
\left| t_{\text{skew,RAND,21}} \right|
$$

Substitue the maximum skew into the Eq. (4) in [#max-delay-constraint](lec-01b-timing-synchronous.md#max-delay-constraint "mention"), the resulting clock cycle T<sub>CK</sub> will be,

$$
T_{\text{CK}}
=
\left(
\tau_{\text{COMB,max}}
-
\tau_{\text{COMB,min}}
\right)
+
\left(
t_{\text{SETUP,REG2}}
+
t_{\text{HOLD,REG2}}
\right)
+
2\left(
\left| t_{\text{skew,RAND,21}} \right|
+
\left| t_{\text{jitter}} \right|
\right)
$$

The following compares the minimum clock period (T<sub>CK</sub>) we can achieve in two different design scenarios, which is either to use skew or not use it at all

| TCK=                  | zero (intentional skew)                                                        | max (intentional skew)                                                                      |
| --------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| **combinational +**   | $$\tau_{\text{COMB,max}}$$                                                     | $$\tau_{\text{COMB,max}} - \tau_{\text{COMB,min}}$$                                         |
| **FF overhead +**     | $$t_{\text{SETUP,REG2}} + \tau_{\text{CK-Q,REG1}}$$                            | $$t_{\text{SETUP,REG2}} + t_{\text{HOLD,REG2}}$$                                            |
| **clocking overhead** | $$\left| t_{\text{skew,RAND,21}} \right| + 2\left| t_{\text{jitter}} \right|$$ | $$2\left(\left| t_{\text{skew,RAND,21}} \right| + \left| t_{\text{jitter}} \right|\right)$$ |

From the table, we can see that if you intentionally design to use a positive skew to improve performance:

* the impact of combinational delay drastically reduced
* FF overhead increased a bit
* doubled impact of random clock skew

{% hint style="info" %}
In the "zero" column, $$t_{\text{skew,DET}} = 0$$. In the "max" column, $$t_{\text{skew,DET}} = t_{\text{skew,DET,max}}$$.
{% endhint %}

<details>

<summary>Sequencing vs. Timing Constraint</summary>

As we have talked about [#sequencing-in-synchronous-systems](lec-01b-timing-synchronous.md#sequencing-in-synchronous-systems "mention") before, now we may find an interesting interpretion that

> timing constraints <-> conditions for correct sequencing

| Sequencing                        | Meaning                                                    | Timing Constraint                                                                     |
| --------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| data moves by **one**             | COMB must complete computation within one cycle            | [#max-delay-constraint](lec-01b-timing-synchronous.md#max-delay-constraint "mention") |
| and **only one** stage each cycle | input of REG 1 does not affect output of REG2 at one event | [#min-delay-constraint](lec-01b-timing-synchronous.md#min-delay-constraint "mention") |

</details>

## Performance in Synchronous VLSI Systems

In Synchronous VLSI systems, **performance** is an application-dependent term.

### Throughput

**Throughput** is defined as the number of computations completed per unit of time.

$$
\text{Throughput} = \frac{\text{Number of Computations}}{\text{Time (seconds)}}
$$

Depending on the application, throughput is measured differently:

* **Generic Digital Logic**: Operations/sec (MOPS - Million Operations Per Second).
* **Microprocessors** ($$\mu$$P): Instructions/sec (MIPS, BIPS).
* **Arithmetic-Intensive (GPU)**: Floating-point ops/sec (FLOPS).
* **Signal Processing (DSP)**: Samples/sec (kSPS).

{% hint style="success" %}
**Throughput** is the primary metric for continuous processing tasks like video streaming, DSP, or servers.&#x20;
{% endhint %}

#### Single Block Throughput Model

When analyzing a specific hardware block (e.g., an accelerator), we assume:

* **Conservation**: Data is not randomly created or lost; every input word generates a specific number of output words.
* **Ideal Flow**: Inputs are applied "just in time" (no stalling).
* **Functionality Factor** ($$X$$): Each input value generates $$X$$ sequential output values.

This leads to the relationship between Input Data Rate ($$DR_{in}$$) and Output Data Rate ($$DR_{out}$$):

$$
DR_{out} = X \cdot DR_{in}
$$

Where $$X$$ is the **Data Rate Gain** set by the function of the block

$$
X = \frac{DR_{out}}{DR_{in}} = \frac{\# \text{words}_{out}}{\# \text{words}_{in}}
$$

<details>

<summary>Example of Data Rate Gain</summary>

The value of $$X$$ tells us if the block is **compressing or expanding** data.

| Case                | Application Example      | Input                               | Output                            | Ratio (X) |
| ------------------- | ------------------------ | ----------------------------------- | --------------------------------- | --------- |
| Compression (X < 1) | 4K Video Compression     | 3840 × 2160 pixels (high bandwidth) | Compressed stream (3:1 reduction) | 1/3       |
| Reduction (X < 1)   | AlexNet (Neural Network) | 224 × 224 pixels (image)            | 1,000 classes (classification)    | 1/50      |
| Transform (X = 1)   | 1024-point FFT           | 1,024 audio samples                 | 1,024 frequency points            | 1         |
| Expansion (X > 1)   | 12-filter FIR Bank       | 6 samples                           | 12 filtered outputs per sample    | 12        |

</details>

#### Cascaded Blcoks & The Bottleneck

Real systems consist of multiple blocks chained together. To find the maximum performance of the entire chain, we must find the "bottleneck" (the **slowest block**).

However, because the data rate changes (expands or compresses) at each stage, we cannot simply compare the raw speeds of Block 1 and Block 2. We must normalize everything to the **Output-Referred Data Rate**, which basically means asking

> If this specific block runs at its maximum speed, how much final output does that equal?

{% stepper %}
{% step %}
#### Single Block Limit

The output of a single block is limited by either the incoming data or its own maximum internal speed ($$maxDR_{out}$$):

$$
\text{DR}_{out} = \min(X \cdot \text{DR}_{in}, \ max\text{DR}_{out})
$$
{% endstep %}

{% step %}
#### Chain Limit (The Bottleneck Formula)

For a chain of blocks ($$1 \to 2 \to \dots \to N$$), the maximum system throughput is the minimum of all blocks' capacities, **scaled to the output**:

$$
maxDR_{out} = \min \left( \underbrace{X_2 \cdot \dots \cdot X_N \cdot maxDR_{out,1}}_{\text{Block 1 limit reflected at output}}, \ \dots \ , \underbrace{maxDR_{out,N}}_{\text{Block N limit}} \right)
$$

1. **For the First Block 1**:
   1. Take its raw max speed ($$maxDR_{out,1}$$).
   2. Multiply it by the gain of Block 2 ($$X_2$$), then Block 3 ($$X_3$$), all the way to Block N.
   3. This result tells us: _"If Block 1 runs at 100%, how much final output is produced?"_
2. **For the Last Block N**: Its limit is just its own max speed ($$maxDR_{out,N}$$).
{% endstep %}
{% endstepper %}

In a perfect design, no block should be faster than necessary. If Block 2 is 10x faster than the system bottleneck, it is "wasted" potential (and wasted silicon area/power). So

{% stepper %}
{% step %}
#### The Goal

Every term inside the min() function should be equal.
{% endstep %}

{% step %}
#### The Math

Ideally, the max speed of any specific block i ($$maxDR_{out,i}$$) should be exactly:

$$
maxDR_{out,i} = \frac{\text{Target System Throughput}}{\prod_{j=i+1}^{N} X_j}
$$

This basically means that a block's designed speed should equal the target final speed divided by all the gains that come after it.
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
#### Practical Bottlenecks

In real-world chips (like the Roofline Model), the bottleneck usually shifts between three areas:

* Compute-Bound: The arithmetic logic (ALU) is too slow.
* Memory-Bound: We cannot read/write data to RAM fast enough.
* Communication-Bound: We cannot move data between cores fast enough.
{% endhint %}

### Latency

> While throughput measures the "volume" of work (how many?), Latency measures the "speed" of a single task (how fast?).

**Latency** is the time required to complete a **single** computation from the moment inputs arrive until the final output is valid.

<figure><img src="../../.gitbook/assets/latency.png" alt=""><figcaption></figcaption></figure>

Latency is measured as:

* Absolute Time: Nanoseconds (ns), milliseconds (ms).
* Clock Cycles: How many "ticks" it takes.

It is critical for real-time systems where a delay is unacceptable, such as autonomous vehicles braking or network packet switching.

#### Throughput vs. Latency Relationship

There is a common misconception that throughput is just the inverse of latency ($$1/\text{Latency}$$). This is only true in the specific case.

{% stepper %}
{% step %}
#### Case A: No Overlap (Serial Execution)

If the system can only accept a new request _after_ the previous one is completely finished, then:

$$
\text{Throughput} = \frac{1}{\text{Latency}}
$$
{% endstep %}

{% step %}
#### Case B: Execution Overlap (Pipelining/Parallelism)

In most modern VLSI systems, we use **pipelining** (as we did in NUS CG3207!). We accept new inputs while the previous ones are still being processed in later stages.

$$
\text{Throughput} \gg \frac{1}{\text{Latency}}
$$
{% endstep %}
{% endstepper %}

#### Calculating System Latency

Unlike throughput (which is limited by the _slowest_ block), latency is additive. We must sum up the time spent in every block along the path.

{% stepper %}
{% step %}
#### Block-Level Latency ($$LAT_i$$)

This is the time taken by a single block to finish its job.

**The Worst-Case Rule**: If the latency is **data-dependent** (e.g., a multiplier takes longer for large numbers than small ones), we must use the Upper Bound ($$\max(LAT_i)$$) to guarantee timing correctness.
{% endstep %}

{% step %}
#### System-Level Latency Formula

For a system with N blocks, the total latency is not just the simple sum. We must account for **loops** or repeated execution of specific blocks.

<figure><img src="../../.gitbook/assets/system-level-latency.png" alt=""><figcaption></figcaption></figure>

$$
\text{Total Latency} \le \sum_{i=1}^{N} (\#execution_i \cdot \max(LAT_i))
$$

* $$\#execution_i$$: The number of times Block i is executed to complete _one_ full system computation.
* $$\max(LAT_i)$$: The worst-case latency of Block i.
{% endstep %}
{% endstepper %}

### Case Study: AlexNet CNN

To understand how throughput and latency principles apply to real-world hardware, we analyze the **AlexNet Convolutional Neural Network (CNN)**. This example demonstrates how data expansion/compression ($$X$$) and computational intensity vary dramatically across different stages of a system.

#### Architecture Overview

AlexNet is a classic CNN used for image classification (mapping a raw image to 1 of 1,000 classes). It consists of a pipeline of layers with distinct functions:

* **CONV (Convolutional Layers)**: The "Trainable" feature extractors. They use kernels ($$k \times k$$) to filter input images.
  * **Dominant Operation**: Multiply-Accumulate (MAC).
  * **Data Flow**: Input Frame ($$227 \times 227$$ RGB) -> Convolved Features.
* **ReLU (Rectified Linear Unit)**: Non-linear activation function ($$output = \max(0, input)$$). Simple and fast.
* **POOL (Max Pooling)**: Down-sampling layer. Reduces data size by taking the maximum value in a patch.
* **FC (Fully Connected Layers)**: The final classification stage. Every input neuron connects to every output neuron (matrix-vector multiplication).

<figure><img src="../../.gitbook/assets/alex-cnn-architecture.png" alt=""><figcaption></figcaption></figure>

#### Throughput Analysis: "The Bottleneck Shift"

{% stepper %}
{% step %}
#### Data Volume vs. Operations

&#x20;($$words_{out}$$): The number of data words _decreases_ as we move deeper into the network.

* _Early Layers (Conv1-Conv2):_ High data volume (hundreds of thousands of words) due to large spatial maps.
* _Later Layers (FC6-FC8):_ Low data volume (thousands of words).

**Computational Load (MACs)**: The number of operations is massive in the middle layers.

* _Conv2:_ Requires 224 Million MACs (Peak Compute Load).
* _FC8:_ Requires only 4.1 Million MACs.

<figure><img src="../../.gitbook/assets/wordsout-mac.png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### Target Throughput Calculation

To sustain a real-time performance of 30 frames per second (fps), each layer must meet a specific throughput target. This target depends on the layer's "Gain" ($$X$$):

$$
\text{Throughput(Layer } i) = \frac{\#ops}{\#words_{out}} \cdot \frac{DR_{out, final}}{\prod_{j=i+1}^{N} X_j}
$$

* Expansion ($$X > 1$$): Layers like Conv1 ($$X=1.89$$) and Conv2 ($$X=2.67$$) expand data, increasing the throughput burden on subsequent blocks.
* Compression ($$X < 1$$): Layers like MaxPool ($$X \approx 0.23$$) and FC6 ($$X=0.44$$) aggressively reduce data rates.

<figure><img src="../../.gitbook/assets/x-factor.png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### The "Roofline" Reality

When we map these requirements to hardware limits, we see distinct bottlenecks:

* **Compute-Bound**: The system is limited by how many MACs it can do per second.
  * _Dominant Layers:_ CONV Layers (require 215 MMACs/s to sustain 30fps).
  * _Implication:_ CNN accelerators are essentially huge arrays of MAC units.
* **Memory-Bound**: (See next section).

<figure><img src="../../.gitbook/assets/total-throughput.png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

#### Memory Bandwidth and Capacity

Throughput is not just about computing; it is about moving data.

{% stepper %}
{% step %}
#### Memory Capacity (Storage)

* **Feature Memory (Intermediate Data)**: Largest at the _start_ (hundreds of KBs for Conv1/Conv2).
* **Weight Memory (Parameters)**: Dominant at the _end_ (FC Layers).
  * **FC6** alone requires **37.7 Million** parameters (Weights/Biases).
  * _Implication:_ FC layers are heavily memory-capacity constrained (often requiring off-chip DRAM).
{% endstep %}

{% step %}
#### Memory Bandwidth (Speed)

To feed the compute units for 30fps, we need high bandwidth:

* Peak Bandwidth: Occurs at FC6 ($$\approx$$ 1,100 MB/s).
* **Trend:**
  * _First Layers:_ Low bandwidth (compute-bound).
  * _Last Layers:_ Massive bandwidth spike (memory-bound) because FC layers read huge weight matrices for relatively few computations.

<figure><img src="../../.gitbook/assets/memory-bandwidth.png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}
