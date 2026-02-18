# Lec 01b - Timing Synchronous

## Synchronous Sequencing Models

### System Abstraction

A digital system consists of communicating blocks shown as follows,

<figure><img src="../../.gitbook/assets/communication-blocks.png" alt="" width="563"><figcaption></figcaption></figure>

We assume that each combinational module evaluates its output in

$$
\tau_{\text{comb}} \in [\tau_{\text{comb},\text{min}},\tau_{\text{comb},\text{max}}]\;(\text{input-dependent})
$$

For example, the module 2 needs to "know" when in<sub>2</sub> / out<sub>1</sub> is correct (has settled). This can be illustrated using the following figure,

<figure><img src="../../.gitbook/assets/communication-between-digital-modules.png" alt="" width="563"><figcaption></figcaption></figure>

In module 1, the <mark style="color:green;">fast path</mark> gives earliest possible correct output ($$\tau_{\text{comb},\text{min}}$$), while the <mark style="color:red;">slow path</mark> gives the latest settling output ($$\tau_{\text{comb},\text{max}}$$). The correct out<sub>1</sub> settles between $$\tau_{\text{comb},\text{min}}$$ and $$\tau_{\text{comb},\text{max}}$$. So, to ensure out<sub>1</sub> is correct, we wait until after $$\tau_{\text{comb},\text{max}}$$. This again highlights the importance of [**critical path**](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#critical-path)!

{% hint style="info" %}
#### Some Notations

Some notations used in timing diagrams:

1. **Cross-hatched (zig-zag) region**: This means that the signal is **unstable**. So, in<sub>1</sub> is stable only after the first dashed line.
2. **Flat solid line:** This means that the signal is **stable**, it can be high or low, but it doesn't matter that much.

These two notations ($$\tau_{\text{comb},\text{min}}$$ and $$\tau_{\text{comb},\text{max}}$$) have appeared in [Harris and Harris's DDCA](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#propagation-and-contamination-delay)! Thus we can also say that

1. $$\tau_{\text{comb},\text{min}}$$ is called the **contamination delay**.
2. $$\tau_{\text{comb},\text{max}}$$ is called the **propagation delay**.
{% endhint %}

#### Synchronous vs. Asynchronous

As shown above, if changes in **out**<sub>**1**</sub> propagate immediately, the second module may observe incorrect or unstable inputs. To prevent this, we introduce two approaches that ensure the second module always receives stable and correct inputs from the first module.

{% stepper %}
{% step %}
#### Asynchronous

In asynchronous mode, additional circuitry generates "completion signal" and this completion signal enables the computation of next block when its output is valid.

<figure><img src="../../.gitbook/assets/asynchronous-communication-mode.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="warning" %}
This approached is **less favored** by the industry. The reason is **not** because this approach is less-efficient. In fact, the asynchronous approach is **more efficient** then the synchronous approach. The reason is that the automation tools don't support asynchronous systems well, but they support synchronous systems well.
{% endhint %}
{% endstep %}

{% step %}
#### Synchronous

In synchronous mode, all input and output signals are synchronized to the clock **clk**. The clock periodically triggers the next computation cycle and provides a global timing reference for all modules in the system.

All I/O signals are only allowed to be _used_ at **clock events**. And all the computation is done during the **clock period**.

* **Clock Event**: At the clock event
  * Registers sample their inputs, and then
  * Registers update their outputs
  * The _next computation cycle begins_

{% hint style="info" %}
Think of the above steps as: “Everyone stop, look at your inputs, remember them, then start computing again.”
{% endhint %}

* **Clock Period**: During the clock period
  * Registers hold stable values at their outputs
  * Combinational logic computes new values
  * Results must be ready **before the next clock event**. If not, will cause timing violation!

{% hint style="success" %}
To understand **sychronize** better, you can go to the [DICADP](../../textbook-1-dicadp/timing-issues-in-digital-circuits/classification-of-digital-systems.md#synchronous-interconnect). Basically, **synchronization** turns "random arrival" into "scheduled arrival" so that the data/input can be sampled directly without any uncertainty. This will create stable input for the following combinational logic to process it during the entire upcoming clock cycle (We have seen this uncertainty from [above](lec-01b-timing-synchronous.md#system-abstraction)).
{% endhint %}

In the following timing diagram, the first time shift represents the **Setup Time** (t<sub>setup</sub>), which is the required window where input signal `x` must be stable before the clock edge to be sampled correctly. The second time shift represents the **Clock-to-Output Delay** (t<sub>CK-Q</sub>), which is the time it takes for the register to react to the clock edge and update the output signal `y` before it propagates to the subsequent combinational logic.

<figure><img src="../../.gitbook/assets/synchronous-communication-mode.png" alt="" width="563"><figcaption></figcaption></figure>

And if we draw a register diagram, it will be similar to the following

<figure><picture><source srcset="../../.gitbook/assets/lec01-synchronous-example-dark.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/lec01-synchronous-example-light.png" alt=""></picture><figcaption></figcaption></figure>

{% hint style="danger" %}
In both cases of synchronous and asynchronous design, energy/timing/area overhead is paid for.
{% endhint %}
{% endstep %}
{% endstepper %}

### The Clock Discipline

#### Sequencing in Synchronous Systems

In digital logic, the order of data flow is critical. Usually, we want to achieve the **Deterministic Sequencing**. Thus we require a strict First-In, First-Out (FIFO) behavior, where the n<sup>th</sup> input produces the n<sup>th</sup> output in the exact same sequence. Ideally, this is easy to achieve if the delay through a module is data independent (e.g., every calculation takes the exact same amount of time).

<figure><img src="../../.gitbook/assets/lec01-sequence-synchronous-data-independent.png" alt=""><figcaption></figcaption></figure>

However, in reality, the time it takes for a signal to propagate through a combinational block depends on the specific input values. As we have seen [above](lec-01b-timing-synchronous.md#system-abstraction) or in [Harris & Harris DDCA](https://wenbo-notes.gitbook.io/ddca-notes/textbook/combinational-logic-design/timing#propagation-and-contamination-delay):

* Propagation Delay (t<sub>pd</sub>): The time taken by the _slowest_ path (critical path).
* Contamination Delay (t<sub>cd</sub>): The time taken by the _fastest_ path (short path).

<figure><img src="../../.gitbook/assets/lec01-sequence-synchronous-data-dependent.png" alt=""><figcaption></figcaption></figure>

If we connect modules directly without synchronization (asynchronous design without handshaking mechanism), "Fast" data can overtake "Slow" data. For example,

1. **Input A (Complex):** We send a Division instruction (Slow Path). It begins processing.
2. **Input B (Simple)**: Immediately after, we send an Add instruction (Fast Path).

Because the Adder logic is much faster (shorter contamination delay) than the Divider logic, the result of the **Add** races through the circuit and appears at the output _before_ the Division is finished. So, the system reads the wrong result order (`Result B` -> `Result A`). This is a **Race Condition**, leading to data corruption and system failure.

To prevent the "race conditions" described previously, we enforce a strict rule:

> Data must move by exactly one stage per clock cycle.

This is also called the **lockedstep movement**.

* This movement is controlled by a global **Clock Event** (usually the rising or falling edge of a signal).
* In a synchronous system, we ignore the transient "noise" or glitches that happen between clock edges.
  * We define the signal's value at the i-th cycle as the _single_ stable value present right[^1] before the sampling edge.
  * Any changes happening _between_ cycle i and i+1 are considered "work in progress" and are ignored until the next clock edge arrives.

<figure><img src="../../.gitbook/assets/lec01-sequencing-with-register.png" alt=""><figcaption></figcaption></figure>

In the diagram above, "**in**<sub>**1**</sub>**(i)**" denotes a signal using a common notation that will be used throughout this module.

* **"in**<sub>**1**</sub>**"** is the signal name, indicating which signal is being referenced, while
* **"(i)"** represents the value of that signal (High or Low) at clock cycle **i**. For example, **in**<sub>**1**</sub>**(i+1)** denotes the value of **in**<sub>**1**</sub> in the next clock cycle relative to **in**<sub>**1**</sub>**(i)**. So, on the L.H.S of in<sub>1</sub>(i) are the future inputs while on the R.H.S of in<sub>1</sub>(i) are the past inputs.

So how do we implement the box labeled “?” so that it holds the output of the first module until the next clock cycle, allowing it to be processed by the second module in the next clock cycle?

The solution is to use flip-flops or latches, which have alreday been introduced in EE2026 and [_Harris & Harris DDCA_](https://wenbo-notes.gitbook.io/ddca-notes/textbook/sequential-logic-design/latches-and-flip-flops)_**.**_

> A D flip-flop **copies D to Q on the rising edge of the clock, and remembers its state at all other times until the next rising edge comes, then it will update its state**.
>
> <p align="right">— Harris &#x26; Harris DDCA</p>

For example, by using flip-flops to implement the box labeled “?”, at clock cycle **i**, it holds the value **in**<sub>**1**</sub>**(i)** constant for the entire cycle (that is, the flip-flop has already sampled **in**<sub>**1**</sub>**(i)** from **D** and is driving it on **Q**). This allows Module 1 to process a stable input. Meanwhile, the next value **in**<sub>**1**</sub>**(i+1)** may arrive at the input **D** at any time during cycle **i**, but it is not visible at **Q** and will only be captured and presented at the rising edge of clock cycle **i+1**.

{% hint style="warning" %}
In the industry, latches are **rarely** used because the **timing constraint** for latches is hard to analyze. In this course, we focus on **positive edge-triggered** FFs and we will use registers and FFs interchangeably.
{% endhint %}

## Clock Network Imperfections

### Distribution Challenges

Ideally, the clock signal is

1. instantaneously distributed to all flip-flops (FFs)
2. periodic

However, as clock is distributed throughout the chip with wires + repeaters:

1. delays in clock distribution network maybe different
2. different **arrival time** t<sub>i</sub> of clock signal clk<sub>i</sub> (same clcok signal but arrives at different FFs at different time)

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

{% hint style="success" %}
In synchronous circuit design, while we originate from a **single global clock source** (CLK),

* clk<sub>i</sub> refers to the local clock signal branch connected to FF<sub>i</sub>.&#x20;
* The variable t<sub>i</sub> denotes the specific time instant when the clock edge actually arrives at FF<sub>i</sub>.
{% endhint %}

When distributing clk in the **same direction** as data flow, the skew t<sub>skew, ij</sub> is positive. If **opposite direction**, the skew is **negative**. For example, the following diagram shows a **positive clock skew** by assuming that the data and clock distribution flows from register R1 to register R2.

<figure><img src="../../.gitbook/assets/clock-skew-sign-example.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="success" %}
An analogy is to think of the clock skew as a **vector** and the two points are t<sub>i</sub> and t<sub>j</sub>. Thus,

<p align="center"><span class="math">t_{\text{skew, ij}}=t_i-t_j=-t_{\text{skew, ji}}</span></p>
{% endhint %}

Another example will be the timing diagram we have seen in [Harris & Harris DDCA](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/sequential-logic-design/timing-of-sequential-logic#clock-skew). The following diagram will indicate a **negative skew** seen by R1 (t<sub>1</sub> - t<sub>2</sub> < 0, assuming the data flows from R1 to R2).

<figure><img src="../../.gitbook/assets/clock-skew-example-ddca.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="warning" %}
The clock skew is relative. But we usually calculate the clock skew **following the direction of the data path**.

* If data flows from register 1 to register 2, we calculate skew as t<sub>2</sub>- t<sub>1</sub>. During the calculation, treat t<sub>2</sub> and t<sub>1</sub> as two **algebric values** and the result will be either negative or positive!
* **Seen by** which register, use that register as the first term. For example, the clock skew seen by register R1 w.r.t. R2 is t<sub>1</sub>-t<sub>2</sub>.
{% endhint %}

#### Clock Skew Calculation

The clock skew can randomly vary due to delay variations, but it can be calculated using the following formula (in EE4415),

$$
t_{\text{skew}} = t_{\text{skew,DET}} \pm \left| t_{\text{skew,RAND}} \right|
$$

From this formula, we can see that the clock skew has two components

1. **Deterministic skew**: Predictable delay caused by the fixed physical layout of wires and repeaters. Since its sign and magnitude are known, it can be intentionally engineered to optimize timing paths (useful skew). This value can be either **positive** or **negative**.
2. **Random skew**: Unpredictable variation arising from manufacturing mismatches or environmental factors like temperature. It creates a bounded uncertainty range ($$\pm$$) with an unknown sign that designers must account for as noise.

### Clock Jitter

In reality, the clock period T<sub>CK</sub> is not perfectly constant due to jitter. Specifically, we define **cycle-to-cycle jitter** (t<sub>jitter</sub>) as the random, time-varying deviation between two successive clock events (such as two rising clock edges). This means the actual clock period is not fixed; instead, it fluctuates around a nominal period (T<sub>nom</sub>), strictly bounded within the range of $$T_{\text{nom}} - |t_{\text{jitter}}|$$ to $$T_{\text{nom}} + |t_{\text{jitter}}|$$.

<figure><img src="../../.gitbook/assets/clock-jitter.png" alt=""><figcaption></figcaption></figure>

#### The Reason of Clock Jitter

The two main reasons for clock jitter are:

1. **clock generator**'s intrinsic jitter
2. **clock distribution network**: due to time-varying delay of repeaters (supply noise)

<figure><img src="../../.gitbook/assets/clock-jitter-cause.png" alt=""><figcaption><p>Clock Distribution Network</p></figcaption></figure>

Based on the above diagram, we have the formula for calculating the t<sub>jitter,i</sub> to be

$$
t_{\text{jitter},i} = t_{\text{jitter,clock\_gen}} + \sum_{j=1}^{n} \left| \Delta \tau_{\text{PD,buffer},j} \right|\tag{5}
$$

<details>

<summary>Clock Jitter Difference</summary>

In a clock distribution network, the clock paths to two registers (FF<sub>i</sub> and FF<sub>j</sub>) share a **common path** and then split into **separate local branches**.

{% hint style="warning" %}
As we have seen in Eq.5, jitter is introduced by buffers, wires, and supply noise along these paths.
{% endhint %}

In EE4415, we assume that the **common path dominates**. This means that most of the jitter is added **before the clock splits**, so both FF<sub>i</sub> and FF<sub>j</sub> see **almost the same clock shift** in the same given clock cycle. In the waveform below, this is represented by the jitter occurring at the **green circle** for both clocks.

<figure><img src="../../.gitbook/assets/clock-jitter-difference-dark.png" alt="" width="369"><figcaption></figcaption></figure>

{% hint style="danger" %}
In different clock cycles, the jitter can shift in either direction and does not have to be the same as in the previous cycle.
{% endhint %}

In reality (but not considered in EE4415), the local branches also add jitter because they have different buffers and power supply noise. This causes **small differences** between clk<sub>1</sub> and clk<sub>2</sub>, so jitter does not occur at exactly the same instant on both clocks and it can happen that jitter will appear at **differnet** direction.

</details>

## Timing Analysis

### Flip-Flop Timing Characteristics

> This section has a lot of similarities with the [Timing of Sequential circuits](https://app.gitbook.com/o/MnEKr5A4lYXtOfhoXGj5/s/jTJFBPtKk6NwweAooH53/) in Harris & Harris DDCA!

In positive-edge triggered (PET) flip flops, input is **sampled** at rising clock edge. And the **timing parameters** for D Flip Flops are:

1. input (usually the old input, will see why it's "old" in [DICADP](../../textbook-1-dicadp/timing-issues-in-digital-circuits/synchronous-design-an-in-depth-perspective.md#synchronous-timing-basic)) must be kept stable from **t**<sub>**SETUP**</sub>**&#x20;before** the active edge to **t**<sub>**HOLD**</sub>**&#x20;after** this edge. Otherwise, we will have **metastability**.
2. CK-Q delay: output is updated at t<sub>CK-Q</sub> after clock edge.

<figure><img src="../../.gitbook/assets/timing-parameters-for-edge-triggered-ffs.png" alt="" width="563"><figcaption></figcaption></figure>

#### Asynchronous Resettable Filp Flops

> This section is partly discussed in the [resettable flip flops](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/sequential-logic-design/latches-and-flip-flops#resettable-flip-flop) in Harris & Harris DDCA!

In asynchronous resettable flip flops, the RESET signal has **higher** priority than the CLK signal. So sometimes the RESET can be used to "mask" (hide or block) the CLK signal causing the fact that the Flip-Flop didn't "see" the clock rise because the RESET was blinding it. Simiarly, its timing parameters are:

1. For normal clock event, reset must be kept stable from **t**<sub>**RECOVERY**</sub>**&#x20;before** clock edge to **t**<sub>**REMOVAL**</sub>**&#x20;after** this edge.

For example, the following diagram shows an active-low asynchronous reset

<figure><img src="../../.gitbook/assets/active-low-asynchronous-reset-ffs.png" alt=""><figcaption></figcaption></figure>

* If we want to enable the clock event, release the RESET button at least t<sub>RECOVERY</sub> before the rising clock edge.
* If we want to ignore the clock event, press and hold (small typo in the figure above) the RESET and don't release until t<sub>REMOVAL</sub> after the rising clock edge.

{% hint style="warning" %}
Even if this is an active-low asynchronous resettable FF, pressing the RESET button will reset the FF, meaning that pressing the RESET button is equivalent to set RESET signal to be 0.
{% endhint %}

### System Timing Constraints

#### Timing Constraints in Synchronous Circuits

> Again, this part is almost the same as [Timing of Sequential Circuits](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/sequential-logic-design/timing-of-sequential-logic) covered in Harris & Harris DDCA. But with some more info added on the **timing overhead** caused by clock skew and clock jitter.

The FF timing constraints imply the system timing constraints. The FF timing constraints have

* Setup Time Constraint
* Hold Time Constraint

While the system timing constraint is more about the global CLK signal speed. So, what [the first sentence](#user-content-fn-2)[^2] says is that the FF timing constraint will affect the speed of the system clock, which is an indispensible part of the system timing constraint. So, when designing a system, we should prevent setup/hold violations.

System timing constraints are affected by

* FF time constraints
* combinational logic timing parameters
* clock non-idealities (skew and jitter)

To start, let's first see an intuitive understanding of FF timing constraints.

<figure><img src="../../.gitbook/assets/intuitive-understand-ff-timing-constraint.png" alt="" width="563"><figcaption></figcaption></figure>

* To meet the setup time constraint, we can think of it as "the computation should be completed before next edge in REG<sub>2</sub>" -> This gives us the **max-delay constraint** for the combinational logic
* To meet the hold time constraint, we can think of it as "the computation must affect REG<sub>2</sub> only after a certain time" -> This gives us the **min-delay constraint** for the combinational logic

{% hint style="danger" %}
In this part, as T<sub>CK</sub> is fixed in the specification, t<sub>setup</sub> and t<sub>ck-q</sub> are constant, we focus on t<sub>comb</sub>!
{% endhint %}

#### Terminology Mapping

As the following parts are mostly covered in [DICADP](../../textbook-1-dicadp/timing-issues-in-digital-circuits/#synchronous-design-an-in-depth-perspective), here is the table summarizing the difference between some terminologies used:

| DICADP (Textbook1)      | EE4415                                                           | Description                                                                                                                                             |
| ----------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $$T_{CLK}$$ or $$T$$    | $$T_{CK}$$                                                       | Nominal clock period                                                                                                                                    |
| $$t_{logic}$$           | $$\tau_{\text{COMB,max}}$$                                       | Maximum propagation delay through combinational logic                                                                                                   |
| $$t_{logic,cd}$$        | $$\tau_{\text{COMB,min}}$$                                       | Minimum contamination delay through combinational logic                                                                                                 |
| $$t_{c-q}$$             | $$\tau_{\text{CK-Q},\text{REG1}}$$                               | Register clock-to-Q propagation delay                                                                                                                   |
| $$t_{c-q, cd}$$         | $$\tau_{\text{CK-Q},\text{REG1}}$$                               | Register clock-to-Q contamination delay (Prof. Massimo says that for registers, the contamination delay and the propagation delay are almost the same.) |
| $$t_{su}$$              | $$t_{\text{SETUP},\text{REG2}}$$                                 | Setup time of the destination register                                                                                                                  |
| $$t_{hold}$$            | $$t_{\text{HOLD},\text{REG2}}$$                                  | Hold time of the destination register                                                                                                                   |
| $$\delta$$ (Clock Skew) | $$t_{\text{skew,DET}} \pm \left|t_{\text{skew,RAND,21}}\right|$$ | Clock skew (deterministic ± random in EE4415 but only deterministic in DICADP)                                                                          |
| $$t_{\text{jitter}}$$   | $$t_{\text{jitter}}$$                                            | Clock jitter                                                                                                                                            |
| /                       | $$\tau_{\text{comb}}$$                                           | Timing of the combinational logic between registers                                                                                                     |
| /                       | $$\tau_{\text{comb,max/min}}$$                                   | Maximum and minimum allowed combinational delay                                                                                                         |

{% hint style="danger" %}
In EE4415, the definition of clock jitter is a bit different from DICADP. So, we must follow EE4415's logic: **Jitter cancels out for Hold Time** because it is a "common mode" noise source in the same clock domain. However, in EE4415 we introduces Random Skew ($$|t_{skew,RAND}|$$) to account for the variation that _does_ differ between the two registers (like thermal noise in the wires), which the DICADP textbook might have just lumped into "jitter." This is further discussed in [#clock-jitter-difference](lec-01b-timing-synchronous.md#clock-jitter-difference "mention").
{% endhint %}

#### Max-Delay Constraint

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
T_{\text{CK}} \ge \tau_{\text{COMB,pd}} + t_{\text{SETUP, REG2}} + \tau_{\text{CK-Q, REG1}} - t_{\text{skew,DET}} + |t_{\text{skew,RAND,21}}| + 2|t_{\text{jitter}}| \\ 
\text{or} \\ 
\tau_{\text{COMB,pd}} \le T_{\text{CK}} - t_{\text{SETUP, REG2}} - \tau_{\text{CK-Q, REG1}} + t_{\text{skew,DET}} - |t_{\text{skew,RAND,21}}| - 2|t_{\text{jitter}}| \tag{2}
$$

<figure><img src="../../.gitbook/assets/max-delay-constraint.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
As the spirit is to make R.H.S as big as possible (worst-case scenario), we will map the clock skew $$\delta$$ to $$t_{\text{skew,DET}} - |t_{\text{skew,RAND}}|$$.
{% endhint %}

To simplify it, we group some terms and get the following

$$
\tau_{COMB,pd} \le T_{CK} - t_{OH} \\ \text{where} \\ t_{\text{OH}}= t_{\text{SETUP, REG2}} + \tau_{\text{CK-Q, REG1}} - t_{\text{skew,DET,21}} + |t_{\text{skew,RAND,21}}| + 2|t_{\text{jitter}}|
$$

{% hint style="info" %}
The R.H.S (T<sub>CK</sub>-t<sub>OH</sub>) is denoted as $$\tau_{\text{COMB,max}}$$.
{% endhint %}

Within the t<sub>OH</sub>, we can further do the following grouping

$$
t_{OH}=t_{\text{OH, REG}}+t_{\text{OH, clocking}} \\ \text{where}\\t_{\text{OH,REG}} = t_{\text{SETUP,REG2}} + \tau_{\text{CK-Q,REG1}},\\t_{\text{OH,clocking}} = \left| t_{\text{skew,RAND,21}} \right|
- t_{\text{skew,DET,21}} + 2 \left| t_{\text{jitter}} \right|
$$

From this grouping, we can see that

* Register overhead tOH, REG always **reduces** available time for computation. Same for random skew and jitter.
* But deterministic skew can **increase** the available time for computation.
{% endtab %}
{% endtabs %}

#### Min-Delay Constraint

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
\tau_{\text{COMB,cd}} + \tau_{\text{CK-Q, REG1}} \ge t_{\text{HOLD,REG2,eq}} \\ \text{where} \\ t_{\text{HOLD,REG2,eq}} = t_{\text{HOLD, REG2}} + t_{\text{skew,DET,21}} + |t_{\text{skew,RAND,21}}| \tag{4}
$$

{% hint style="warning" %}
Ideally, we want a register to have **low** hold time (t<sub>hold</sub>). So, to map this to the Eq. 3, the worst-case scenario is when t<sub>HOLD, REG2, eq</sub> is **biggest**, thus we replace $$\delta$$ with $$t_{\text{skew,DET,21}} + |t_{\text{skew,RAND,21}}|$$.
{% endhint %}

<figure><img src="../../.gitbook/assets/min-delay-constraint.png" alt=""><figcaption></figcaption></figure>

Rearrange it, we will get,

$$
\tau_{\text{COMB,cd}} \ge t_{\text{HOLD,REG2,eq}} - \tau_{\text{CK-Q,REG1}}
$$

{% hint style="info" %}
Again, the R.H.S is denoted as $$\tau_{\text{COMB,min}}$$. And we define the difference between the contamination delay of the combinational logic $$\tau_{\text{COMB,cd}}$$ and $$\tau_{\text{COMB,MIN}}$$ as the **robustness margin against hold violations**. ( $$\text{margin}=\tau_{\text{COMB,cd}}-\tau_{\text{COMB,min}}$$)
{% endhint %}

So, we know for sure that

* larger $$t_{\text{HOLD,REG2,eq}}$$ will make $$\tau_{\text{COMB,min}}$$ larger, thus **decreasing** the robust margin **against** hold violations.
* Random skew always **decreases** the robustness as well.
* **Negative** deterministic skew can **improve** the robustness.
{% endtab %}
{% endtabs %}

### Intentional Skew

As we have seen in DICADP, positive skew will improve **performance** but decrease hold robustness while negative skew will decrease **performance** but improve hold robustness.

{% hint style="success" %}
The rule above always holds!
{% endhint %}

As positive skew can improve performance, but it might violate the hold time constraint of R2. So, we will find the **maximum positive skew** we can have for a **designed combinational logic** (meaning that $$\tau_{\text{COMB,cd}}$$ and $$\tau_{\text{COMB,pd}}$$ are fixed). From the Eq. (2) in [#min-delay-constraint](lec-01b-timing-synchronous.md#min-delay-constraint "mention"), we can derive the maximum positive skew as follows,

$$
t_{\text{skew,DET,max}}
=
\tau_{\text{COMB,cd}}
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
\tau_{\text{COMB,pd}}
-
\tau_{\text{COMB,cd}}
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

{% hint style="danger" %}
We can apply two worst case scenario analysis here to get the maximum t<sub>skew, DET</sub> and T<sub>CK</sub>. So, it's not necessary to change the $$\tau_{\text{COMB}}$$ to $$\tau_{\text{COMB,cd}}$$ in the **min-delay constraint** and $$\tau_{\text{COMB,pd}}$$ in the **max-delay constraint**.
{% endhint %}

The following compares the minimum clock period (T<sub>CK</sub>) we can achieve in two different design scenarios, which is either to use skew or not use it at all

| TCK=                  | zero (intentional skew)                                                        | max (intentional skew)                                                                      |
| --------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| **combinational +**   | $$\tau_{\text{COMB,pd}}$$                                                      | $$\tau_{\text{COMB,pd}} - \tau_{\text{COMB,cd}}$$                                           |
| **FF overhead +**     | $$t_{\text{SETUP,REG2}} + \tau_{\text{CK-Q,REG1}}$$                            | $$t_{\text{SETUP,REG2}} + t_{\text{HOLD,REG2}}$$                                            |
| **clocking overhead** | $$\left| t_{\text{skew,RAND,21}} \right| + 2\left| t_{\text{jitter}} \right|$$ | $$2\left(\left| t_{\text{skew,RAND,21}} \right| + \left| t_{\text{jitter}} \right|\right)$$ |

From the table, we can see that if we intentionally design to use a **positive skew** to improve **performance:**

* the impact of combinational delay drastically reduced
* FF overhead increased a bit
* doubled impact of random clock skew

{% hint style="info" %}
In the "zero" column, $$t_{\text{skew,DET}} = 0$$. In the "max" column, $$t_{\text{skew,DET}} = t_{\text{skew,DET,max}}$$. The sum of the "max" column is **always smaller** than the sum of the "zero" column.
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

#### Add skew to clock

In a clock distribution network, we can intentionally add **clock skew** between [**two**](#user-content-fn-3)[^3] registers to improve timing.

{% hint style="warning" %}
The skew can be **positive** or **negative**, depending on whether we want to relax setup or hold constraints.
{% endhint %}

In a real chip, there are many register pairs, and different parts of the clock tree can be tuned to introduce **different skews** in different regions. This is done by **adjusting the delay of clock buffers or repeaters**, for example by:

* Using buffers of different sizes
* Inserting extra delay cells
* Adjusting the loading or wire length
* In some designs, slightly tuning the buffer supply or body bias

By increasing or decreasing the delay of a branch of the clock tree, the clock edge is shifted in time, which creates the desired **intentional skew** between registers.

## Performance Metrics

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

This leads to the relationship between Input Data Rate ($$\text{DR}_{\text{in}}$$) and Output Data Rate ($$\text{DR}_{\text{out}}$$):

$$
\text{DR}_{\text{out}} = X \cdot \text{DR}_{\text{in}}
$$

Where $$X$$ is the **Data Rate Gain** set by the function of the block

$$
X = \frac{\text{DR}_{\text{out}}}{\text{DR}_{\text{in}}} = \frac{\# \text{words}_{\text{out}}}{\# \text{words}_{\text{in}}}
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

However, the data rate may expand or compress at each stage, so we cannot directly compare the raw speeds of Block 1 and Block 2. Instead, we must normalize all blocks to a common metric: the **output-referred data rate**. That is, for each block, we ask:

> if this block operates at its maximum speed, what data rate would that correspond to at the final output of the chain?

{% stepper %}
{% step %}
#### Single Block Limit

The output of a single block is limited by either the **incoming data** or its own maximum internal speed ($$\max\text{DR}_{\text{out}}$$):

$$
\text{DR}_{\text{out}} = \min(X \cdot \text{DR}_{\text{in}}, \max\text{DR}_{\text{out}})
$$

{% hint style="warning" %}
This is true because if a block’s input data rate is low, its output data rate cannot reach the maximum.
{% endhint %}
{% endstep %}

{% step %}
#### Chain Limit (The Bottleneck Formula)

For a chain of blocks ($$1 \to 2 \to \dots \to N$$), the maximum system throughput is the [**minimum**](#user-content-fn-4)[^4] of all blocks' capacities[^5], **scaled to the output**:

$$
\max \text{DR}_{\text{out}} = \min \left( \underbrace{X_2 \cdot \dots \cdot X_N \cdot maxDR_{out,1}}_{\text{Block 1 limit reflected at output}}, \ \dots \ , \underbrace{\max\text{DR}_{\text{out,N}}}_{\text{Block N limit}} \right)
$$

1. **For the First Block 1**:
   1. Take its raw max speed ($$\max\text{DR}_{\text{out,1}}$$).
   2. Multiply it by the gain of Block 2 ($$X_2$$), then Block 3 ($$X_3$$), all the way to Block N.
   3. This result tells us: _"If Block 1 runs at 100%, how much final output is produced?"_
2. **For the Last Block N**: Its limit is just its own max speed ($$\max\text{DR}_{\text{out,N}}$$).
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

Ideally, the max speed of any specific block i ($$\max\text{DR}_{\text{out,i}}$$) should be exactly:

$$
\max\text{DR}_{\text{out,i}} = \frac{\text{Target System Throughput}}{\prod_{j=i+1}^{N} X_j}
$$

This basically means that a block's designed speed should equal the target final speed divided by all the gains that come after it.
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
#### Practical Bottlenecks

In real-world chips (like the Roofline Model), the bottleneck usually shifts between three areas:

* **Compute-Bound**: The arithmetic logic (ALU) is too slow.
* **Memory-Bound**: We cannot read/write data to RAM fast enough.
* **Communication-Bound**: We cannot move data between cores fast enough.
{% endhint %}

### Latency

> While throughput measures the "volume" of work (how many?), **Latency** measures the "speed" of a **single task** (how fast?).

**Latency** is the time/clock cycles required to complete a **single** computation from the moment inputs arrive until the final output is valid.

<figure><img src="../../.gitbook/assets/latency.png" alt=""><figcaption></figcaption></figure>

Latency is measured as:

* **Absolute Time**: Nanoseconds (ns), milliseconds (ms), or
* **Clock Cycles**: How many "ticks" it takes.

It is critical for real-time systems where a delay is unacceptable, such as autonomous vehicles braking or network packet switching.

{% hint style="success" %}
Latency is the similar to the CPI (Clock cycles Per Instruction) we have learned in CG3207!
{% endhint %}

In Lec 02, when performing PPA analysis, we will encounter latency analysis again. Since latency can be expressed in two forms — clock cycles and absolute time — there are two corresponding methods to analyze it:

1. For latency measured in clock cycles, we simply count how many registers have been added, since each register introduces an additional pipeline stage and increases the latency by one clock cycle.
2. For latency measured in absolute time, we need to use mathematical formulas to derive the relationship between the number of registers added and the resulting change in latency. This analysis takes into account both the clock period and the number of pipeline stages to determine the overall time delay.

#### Throughput vs. Latency Relationship

There is a common misconception that throughput is just the inverse of latency ($$1/\text{Latency}$$). This is only true in the specific case.

{% stepper %}
{% step %}
#### Case A: No Overlap (Serial Execution)

If the system can only accept a new request _after_ the previous one is completely finished, then:

$$
\text{Throughput} = \frac{1}{\text{Latency}}
$$

{% hint style="warning" %}
Here, we use **absolute time** to measure **latency**. If **clock cycles** are used to measure the latency, then we should replace "latency" with "latency x cycle-time". Same for below.
{% endhint %}
{% endstep %}

{% step %}
#### Case B: Execution Overlap (Pipelining/Parallelism)

In most modern VLSI systems, we use **pipelining** (as we did in NUS CG3207!). We accept new inputs while the previous ones are still being processed in later stages.

$$
\text{Throughput} \gg \frac{1}{\text{Latency}}
$$

{% hint style="danger" %}
According to [De Micheli](https://wenbo-notes.gitbook.io/ee4218-hsd-notes/textbook-micheli/introduction/computer-aided-synthesis-and-optimization#throughput), the **maximum** throughput of a pipelined system is $$\frac{1}{\text{cycle-time}}$$, which holds under the assumption of a single-issue pipeline (CPI <i class="fa-greater-than-equal">:greater-than-equal:</i> 1). In some microarchitectures such as superscalar processors, where the effective CPI can be less than 1, the maximum throughput may be higher.
{% endhint %}
{% endstep %}
{% endstepper %}

#### Calculating System Latency

Unlike throughput (which is limited by the _slowest_ block), latency is additive. We must sum up the time spent in every block along the path.

{% stepper %}
{% step %}
#### Block-Level Latency ($$\text{LAT}_i$$)

This is the time taken by a single block to finish its job.

**The Worst-Case Rule**: If the latency is **data-dependent** (e.g., a multiplier takes longer for large numbers than small ones), we must use the Upper Bound ($$\max(\text{LAT}_i)$$) to guarantee timing correctness.
{% endstep %}

{% step %}
#### System-Level Latency Formula

For a system with N blocks, the total latency is not just the simple sum. We must account for **loops** or repeated execution of specific blocks.

<figure><img src="../../.gitbook/assets/system-level-latency.png" alt=""><figcaption></figcaption></figure>

$$
\text{Total Latency} \le \sum_{i=1}^{N} (\text{\#execution}_i \cdot \max(\text{LAT}_i))
$$

* $$\text{\#execution}_i$$: The number of times Block i is executed to complete _one_ full system computation.
* $$\max(\text{LAT}_i)$$: The worst-case latency of Block i.
{% endstep %}
{% endstepper %}

### Case Study: AlexNet CNN

{% hint style="danger" %}
Based on my seniors saying, this part won't appear in midterms or finals. It's just a FYI thing. Will see if this still holds in AY25/26 Sem2's EE4415 🤣.
{% endhint %}

To understand how throughput and latency principles apply to real-world hardware, we analyze the **AlexNet Convolutional Neural Network (CNN)**. This example demonstrates how data expansion/compression ($$X$$) and computational intensity vary dramatically across different stages of a system.

#### Architecture Overview

AlexNet is a classic CNN used for image classification (mapping a raw image to 1 of 1,000 classes). It consists of a pipeline of layers with distinct functions:

* **CONV (Convolutional Layers)**: The "Trainable" feature extractors. They use kernels ($$k \times k$$) to filter input images.
  * **Dominant Operation**: Multiply-Accumulate (MAC).
  * **Data Flow**: Input Frame ($$227 \times 227$$ RGB) -> Convolved Features.
* **ReLU (Rectified Linear Unit)**: Non-linear activation function ($$\text{output} = \max(0, \text{input})$$). Simple and fast.
* **POOL (Max Pooling)**: Down-sampling layer. Reduces data size by taking the maximum value in a patch.
* **FC (Fully Connected Layers)**: The final classification stage. Every input neuron connects to every output neuron (matrix-vector multiplication).

<figure><img src="../../.gitbook/assets/alex-cnn-architecture.png" alt=""><figcaption></figcaption></figure>

#### Throughput Analysis: "The Bottleneck Shift"

{% stepper %}
{% step %}
#### Data Volume vs. Operations

&#x20;($$\text{words}_{\text{out}}$$): The number of data words _decreases_ as we move deeper into the network.

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
\text{Throughput(Layer } i) = \frac{\text{\#ops}}{\text{\#words}_{\text{out}}} \cdot \frac{\text{DR}_{\text{out, final}}}{\prod_{j=i+1}^{N} X_j}
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

[^1]: means "immediately" here, not mean left or right

[^2]: The FF timing constraints imply the system timing constraints.

[^3]: Our focus is always **two** registsers.

[^4]: This minimum identifies the **bottleneck**, i.e., the block that limits the overall throughput of the system.

[^5]: "If this block operates at its maximum speed, what data rate would that correspond to at the final output of the chain?"
