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

### Max-Delay Constraint

{% hint style="warning" %}
In this section, let $$ $t_1$ $$t<sub>1</sub> denote the time of the **launching clock edge** at the source register and $$ $t_2$ $$t<sub>2</sub> denote the time of the **capturing clock edge** at the destination register.
{% endhint %}

Using the intuitive understanding of the setup time constraint from above, we can see clearly from the diagram below that:

<figure><img src="../../.gitbook/assets/max-delay-constraint.png" alt=""><figcaption></figcaption></figure>

$$
\begin{align*}
% Setup Constraint Derivation
% Left Side: Data Arrival Time (start time + clock-to-q delay + logic delay)
% Right Side: Data Required Time (next clock edge - setup time)
\tau_{\text{COMB}} + t_{\text{SETUP,REG2}} - \tau_{\text{CK-Q,REG1}}&\le T_{\text{CK}}  \\

% Final inequality for Maximum Combinational Delay
\tau_{\text{COMB,max}} &\le T_{\text{CK}} - t_{\text{SETUP,REG2}} - \tau_{\text{CK-Q,REG1}} \\

% Rearranging to isolate Combinational Delay
% Note: t2 - t1 is substituted with T_CK (Clock Period)
\tau_{\text{COMB}} &\le \underbrace{t_2 - t_1}_{T_{\text{CK}}} - t_{\text{SETUP,REG2}} - \tau_{\text{CK-Q,REG1}} \\
\end{align*}
$$

Adding the consideration of the clock non idealities, we will derive the **worst-case** t<sub>2</sub>-t<sub>1</sub> so that the inequality above will always hold if the T<sub>COMB</sub> satisfies the worst-case scenario.

To make t<sub>2</sub>-t<sub>1</sub> the smallest, given that we already now the diagram as above, we may want

1. t<sub>skew</sub> to be the smallest, although it is always a positive term
2. we want the two clock jitters to behave in the <mark style="color:blue;">blue</mark> way

In the [#clock-skew](lec-01b-timing-synchronous.md#clock-skew "mention") section, we have already seen that t<sub>skew</sub> = t<sub>skew, det</sub> <i class="fa-plus-minus">:plus-minus:</i> |t<sub>skew, rand</sub>|. So, to get the smallest value, we will take the **minus** sign. Thus, our t<sub>2</sub>-t<sub>1</sub> will be as follows:

$$
t_2 - t_1 = T_{\text{CK}} + t_{\text{skew,DET,21}} - \left| t_{\text{skew,RAND,21}} \right| - 2 \left| t_{\text{jitter}} \right|
$$

Substitute the t<sub>2</sub>-t<sub>1</sub> into the inequality above and rearrange it, we will have

$$
\begin{align*}
\tau_{\text{COMB}} &\leq T_{\text{CK}} - \left( t_{\text{SETUP,REG2}} + \tau_{\text{CK-Q,REG1}} \right) \\
&\quad - \left( \left| t_{\text{skew,RAND,21}} \right| - t_{\text{skew,DET,21}} + 2 \left| t_{\text{jitter}} \right| \right)
\end{align*}
$$

The largest value for T<sub>COMB</sub> is denoted as T<sub>COMB, MAX</sub> and it is the maximum value of the propagation delay of the combinational logic. We can further group the last two terms into one term called t<sub>OH</sub> (the overhead time), we can simplify our formula as follows

$$
\tau_{\text{COMB, max}}=T_{\text{CK}}-t_{\text{OH}}
$$

The timing overhead t<sub>OH</sub> reduces available time or the combinational logic and it can be further divided into register and clocking overhead:

$$
t_{\text{OH}}=t_{\text{OH, REG}}+t_{\text{OH, clocking}}
$$

where,

$$
\begin{align*}
t_{\text{OH,REG}} &= t_{\text{SETUP,REG2}} + \tau_{\text{CK-Q,REG1}} \\[1em]
t_{\text{OH,clocking}} &= \left| t_{\text{skew,RAND,21}} \right| - t_{\text{skew,DET,21}} + 2 \left| t_{\text{jitter}} \right|
\end{align*}
$$

Within the whole thing we have discussed till now,

* T<sub>CK</sub> is usually set as system specification
* register overhead always reduce available time for computation
* same for random skew/jitter, but **positive deterministic skew can increase the available time for computation!**

To fix the max-delay violations, T<sub>CK</sub> can be increased.

### Min-Delay Constraint

{% hint style="warning" %}
In this section, let $$ $t_1$ $$t<sub>1</sub> denote the time of the **launching clock edge** at the source register and $$ $t_2$ $$t<sub>2</sub> denote the time of the **launching clock edge** at the destination register.
{% endhint %}

<figure><img src="../../.gitbook/assets/min-delay-constraint.png" alt=""><figcaption></figcaption></figure>

Using the hold time constraint, we can see from the above diagram that the follwoing formula must hold. Otherwise, the hold time constraint is violated

$$
\tau_{\text{CK-Q, REG1}}+\tau_{\text{COMB}}\geq t_{\text{skew, 21}}+t_{\text{HOLD, REG2}}
$$

Substitute t<sub>skew, 21</sub> with t<sub>2</sub>-t<sub>1</sub> and rearrange the formula, we can get

$$
\tau_{\text{COMB}}\geq t_2-t_1+t_{\text{HOLD, REG2}}-\tau_{\text{CK-Q, REG1}}
$$

Let's denote the value of R.H.S as $$\tau_{\text{COMB, min}}$$. Similarly, let's do the worst-case scenario analysis. This time we want to make the $$\tau_{\text{COMB, min}}$$ as big as possible, so

$$
t_2-t_1= t_{\text{skew,DET,21}} + \left| t_{\text{skew,RAND,21}} \right|
$$

{% hint style="danger" %}
As we assume that REG1 and REG2 are in the same clock domain, the clock jitter has no effect (The edge is shifted by same amount in clk<sub>1</sub>)
{% endhint %}

After substitution, we will group the first three terms into one term called t<sub>HOLD, REG2, eq</sub>, this is called the **equivalent hold time of REG2** but extended by including clock non idealities.

$$
t_{\text{HOLD,REG2,eq}} = t_{\text{HOLD,REG2}} + t_{\text{skew,DET,21}} + \left| t_{\text{skew,RAND,21}} \right|
$$

Using t<sub>HOLD, REG2, eq</sub>, we can simplify our formula as follows:

$$
\tau_{\text{COMB,min}} = t_{\text{HOLD,REG2,eq}} - \tau_{\text{CK-Q,REG1}}
$$

Up till now, we can see that

* The robustness margin $$\tau_{\text{CD}}-\tau_{\text{COMB, min}}$$ will indicate whether the design have violated hold time constraint or not
  * If the margin is positive, no hold time violation. Otherwise, got hold time violation
  * $$\tau_{\text{CD}}$$ is the **contamination delay**, it is the actual time when the input signal travels through the fast path in the combinational logic
  * $$\tau_{\text{\text{COMB, min}}}$$ is the **minimum required time requirement** and the combinational logic's contamination delay must be at least this value for it to not violate hold time constraint
* robustness will be degraded by **larger** t<sub>HOLD, REG2, eq</sub>,
* **clock jitter** has no impact but **random clock skew** always degrades robustness
* **negative deterministic clock skew** can improve robustness

Unlike the setup time constraint, the hold time constraint **cannot** be fixed by increasing T<sub>CK</sub>.
