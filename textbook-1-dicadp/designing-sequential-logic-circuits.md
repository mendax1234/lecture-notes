# Designing Sequential Logic Circuits

## Introduction

Combinational logic circuits, described earlier, have the property that the output of a logic block is only a function of the current input values, assuming that enough time has elapsed for the logic gates to settle. Yet virtually all useful systems require storage of state information, leading to another class of circuits called sequential logic circuits. In these circuits, the output not only depends upon the current values of the inputs, but also upon preceding input values. In other words, a sequential circuit remembers some of the past history of the system — it has memory.

### Timing Metrics for Sequential Circuits

There are three important timing parameters associated with a register as illustrated in the Figure below

1. The **set-up** time (t<sub>su</sub>) is the time that the data inputs (D input) must be valid before the clock transition (this is, the 0 to 1 transition for a positive edge-triggered register).
2. The **hold time** (t<sub>hold</sub>) is the time the data input must remain valid after the clock edge.
3. Assuming that the set-up and hold-times are met, the data at the D input is copied to the Q output after a worst-case propagation delay (with reference to the clock edge) denoted by **t**<sub>**c-q**</sub>.

<figure><img src="../.gitbook/assets/register-setup-hold-c2q-time.png" alt=""><figcaption><p><strong>Figure 10.5</strong> Definition of set-up time, hold time and propagation delay of a synchronous register</p></figcaption></figure>

Given the timing information for the registers and the combination logic, some **system-level timing constraints** can be derived. Assume that the worst-case propagation delay of the logic equals t<sub>plogic</sub>, while its minimum delay (also called the contamination delay) is t<sub>cd</sub>. The minimum clock period T, required for proper operation of the sequential circuit is given by

$$
T\geq t_{\text{c-q}}+t_{\text{plogic}}+t_{\text{su}}
$$

The **hold time** of the register imposes an extra constraint for proper operation,

$$
t_{\text{cdregister}}+t_{\text{cdlogic}}\geq t_{\text{hold}} \text{ ,or}\\t_{\text{c-q, min}}+t_{\text{cd}}\geq t_{\text{hold}}
$$

where t<sub>cdregister</sub> / t<sub>c-q,min</sub> is the minimum propagation delay (or contamination delay) of the register and t<sub>cdlogic</sub> / t<sub>cd</sub> is the minimum propagation delay (or contamination delay) of the combinational logic.

$$
t_{\text{hold}}<t_{\text{(c-q, cd)}}+t_{\text{(logic, cd)}}
$$

The above analysis is simplistic since the **clock** is never ideal. As a result of **process** and **environmental variations**, the **clock signal** can have **spatial** and **temporal variations**.

#### Clock Skew

The **spatial variation** in arrival time of a **clock transition** on an integrated circuit is commonly referred to as **clock skew**. The **clock skew** between two points i and j on an IC is given by $$\delta(i, j)=t_i-t_j$$, where t<sub>i</sub> and t<sub>j</sub> are the position of the rising edge of the clock **with respect to a reference**.

{% hint style="warning" %}
$$\delta$$, t<sub>i</sub> and t<sub>j</sub> are all scalars! Treat them just as a signed number!
{% endhint %}

Consider the transfer of data between registers R1 and R2 in **Figure 10.5**. The **clock skew** can be **positive or negative** depending upon the routing direction and position of the **clock source**. The **timing diagram** for the case with **positive skew** is shown in **Figure 10.6**. As the figure illustrates, the **rising clock edge** is delayed by a positive $$\delta$$ at the second register.

<figure><img src="../.gitbook/assets/timing-diagram-with-positive-clock-skew.png" alt=""><figcaption><p><strong>Figure 10.6</strong> Timing diagram to study the impact of clock skew on performance and functionality. In this sample timing diagram, <span class="math">\delta>0</span></p></figcaption></figure>

**Clock skew** is caused by static path-length mismatches in the **clock** load and by definition **skew** is constant from cycle to cycle. That is, if in one cycle **CLK**<sub>**2**</sub> lagged **CLK**<sub>**1**</sub> by $$\delta$$, then on the next cycle it will lag it by the same amount.

{% hint style="warning" %}
It is important to note that **clock skew** does not result in **clock period** variation, but rather **phase shift**.
{% endhint %}

**Skew** has strong implications on **performance** and **functionality** of a **sequential system**. In the following part, we will see how **positive** and **negative skew** will affect the **performance** and **functionality** of a sequential system.

{% hint style="warning" %}
Performance depends on the **setup time constraint** and functionality depends on the **hold time constraint**.
{% endhint %}

{% stepper %}
{% step %}
#### Positive Skew ($$\delta>0$$)

First consider the impact of **positive clock skew** on **performance**. From **Figure 10.6**, a new input **In** sampled by R<sub>1</sub> at edge <i class="fa-circle-1">:circle-1:</i> will propagate through the **combinational logic** and be sampled by R<sub>2</sub> on edge <i class="fa-circle-4">:circle-4:</i>. If the **clock skew** is positive, the time available for signal to propagate from R<sub>1</sub> to R<sub>2</sub> is **increased** by the skew $$\delta$$. Thus, our **effective allowable computing range** will be (T<sub>CLK</sub> + $$\delta$$). The output of the **combinational logic** must be valid one **set-up time** before the **rising edge** of **CLK**<sub>**2**</sub> (point <i class="fa-circle-4">:circle-4:</i>). The constraint on the **minimum clock period** can then be derived as:

$$
T + \delta \geq t_{c-q} + t_{logic} + t_{su} \quad \text{or} \quad T \geq t_{c-q} + t_{logic} + t_{su} - \delta
$$

The above equation suggests that **clock skew** actually has the potential to improve the **performance** of the circuit. That is, the **minimum clock period** required to operate the circuit reliably reduces with increasing **clock skew**! This is indeed correct, but unfortunately, increasing **skew** makes the circuit more susceptible to **race conditions** and may harm the correct operation (functionality) of **sequential systems**.

As above, assume that input **In** is sampled on the **rising edge** of **CLK**<sub>**1**</sub> at edge <i class="fa-circle-1">:circle-1:</i> into R<sub>1</sub>. The new values at the output of R<sub>1</sub> propagates through the **combinational logic** and should be valid before edge <i class="fa-circle-2">:circle-2:</i> at CLK<sub>2</sub>. However, if the **minimum delay** of the **combinational logic** block is small, the inputs to R<sub>2</sub> may change before the **clock edge**, resulting in incorrect evaluation. To avoid **races**, we must ensure that the **minimum propagation delay** through the **register** and **logic** must be long enough such that the inputs to R<sub>2</sub> are valid for a **hold time** after edge <i class="fa-circle-2">:circle-2:</i>. Thus, our **effective hold range** will be (t<sub>hold</sub> + $$\delta$$)The constraint can be formally stated as

$$
\begin{gathered}
\delta + t_{hold} < t_{(c-q, \, cd)} + t_{(logic, \, cd)} \\
\text{or} \\
\delta < t_{(c-q, \, cd)} + t_{(logic, \, cd)} - t_{hold}
\end{gathered}
$$
{% endstep %}

{% step %}
#### Negative Skew ($$\delta<0$$)

Figure 10.7 shows the timing diagram for the case when $$\delta<0$$.

<figure><img src="../.gitbook/assets/timing-diagram-with-negative-clock-skew.png" alt=""><figcaption><p><strong>Figure 10.6</strong> Timing diagram to study the impact of clock skew on performance and functionality. In this sample timing diagram, <span class="math">\delta&#x3C;0</span></p></figcaption></figure>

For its impact on performance, the **rising edge** of CLK<sub>2</sub> happens before the **rising edge** of **CLK1**. On the **rising edge** of CLK<sub>1</sub>, a new input is sampled by R<sub>1</sub>. The new sampled data propagates through the **combinational logic** and is sampled by R<sub>2</sub> on the **rising edge** of CLK<sub>2</sub>, which corresponds to edge <i class="fa-circle-2">:circle-2:</i>. As can be seen from **Figure 10.7** and equation from the performance analysis above, a **negative skew** directly impacts the **performance** of **sequential system**, making the **minimum clock period** for the system to be larger.

However, a **negative skew** implies that the system never fails, since edge <i class="fa-circle-2">:circle-2:</i> happens before edge <i class="fa-circle-1">:circle-1:</i>! This can also be seen from equation from the functionality analysis above, which is always satisfied since $$\delta<0$$. This is because in an ideal clock situation, we have

$$
t_{\text{hold}}<t_{\text{(c-q, cd)}}+t_{\text{(logic, cd)}}
$$

And in this case, our $$\delta<0$$, which means adding a negative to the L.H.S of the inequality above will make it always hold!

{% hint style="warning" %}
In whichever case, the newly derived two time constraints (setup and hold) are the same for both positive and negative skew. Recall that we treat $$\delta$$ as a **scalar**! So the equation doesn't change, it's just the value of $$\delta$$ might change!
{% endhint %}
{% endstep %}
{% endstepper %}

Example scenarios for positive and negative clock skew are shown in Figure 10.8.

<figure><img src="../.gitbook/assets/positive-and-negative-clock-skew.png" alt=""><figcaption><p><strong>Figure 10.8</strong> Positive and egative clock skew</p></figcaption></figure>

{% stepper %}
{% step %}
#### Positive Skew ($$\delta>0$$)

This corresponds to a clock routed in the **same direction** **as the flow of the data** through the pipeline (Figure 10.8a). In this case, the **skew** has to be strictly controlled and satisfy the following equation,

$$
\begin{gathered}
\delta + t_{hold} < t_{(c-q, \, cd)} + t_{(logic, \, cd)} \\
\text{or} \\
\delta < t_{(c-q, \, cd)} + t_{(logic, \, cd)} - t_{hold}
\end{gathered}
$$

If this constraint is not met, the circuit does malfunction independent of the **clock period**.Reducing the **clock frequency** of an **edge-triggered circuit** does not help get around **skew** problems! On the other hand, **positive skew** increases the **throughput** of the circuit as expressed by the following equation,

$$
T + \delta \geq t_{c-q} + t_{logic} + t_{su} \quad \text{or} \quad T \geq t_{c-q} + t_{logic} + t_{su} - \delta
$$

because the **clock period** can be shortened by $$\delta$$. The extent of this improvement is limited as large values of $$\delta$$ soon provoke violations of the first equation (hold time constraint).
{% endstep %}

{% step %}
#### Negative Skew ($$\delta<0$$)

When the clock is routed in the **opposite direction of the data** (Figure 10.8b), the **skew** is **negative** and the following equation (condition) is unconditionally met.

$$
\begin{gathered}
\delta + t_{hold} < t_{(c-q, \, cd)} + t_{(logic, \, cd)} \\
\text{or} \\
\delta < t_{(c-q, \, cd)} + t_{(logic, \, cd)} - t_{hold}
\end{gathered}
$$

The circuit operates correctly independent of the **skew**. The **skew** reduces the time available for actual computation so that the **clock period** has to be increased by $$\left|\delta\right|$$. In summary, routing the clock in the **opposite direction** of the data avoids disasters but hampers the circuit **performance**.
{% endstep %}
{% endstepper %}

Unfortunately, since a general **logic circuit** can have data flowing in **both direction**s (for example, circuits with **feedback**), this solution to eliminate **races** will not always work (**Figure 10.9**).

<figure><img src="../.gitbook/assets/datapath-structure-with-feedback.png" alt=""><figcaption><p><strong>Figure 10.9</strong> Datapath structure with feedback</p></figcaption></figure>

The **skew** can assume both **positive** and **negative** values depending on the direction of the **data transfer**. Under these circumstances, the designer has to account for the **worst-case skew** condition. In general, routing the **clock** so that only **negative skew** occurs is not feasible. Therefore, the design of a **low-skew clock network** is essential.

<details>

<summary><strong>Example</strong>: <strong>Propagation and Contamination Delay Estimation</strong></summary>

{% hint style="warning" %}
This is a very classic question!
{% endhint %}

Consider the logic network shown in Figure 10.10. Determine the propagation and contamination delay of the network, assuming that the worst case gate delay is t<sub>gate</sub>. The maximum and minimum delays of the gates is made, as they are assumed to be identical.

<figure><img src="../.gitbook/assets/example-pd-cd.png" alt=""><figcaption><p><strong>Figure 10.10</strong> Logic network for computation of performance</p></figcaption></figure>

**Sol:** The **contamination delay** is given by **2 t**<sub>**gate**</sub> (the delay through **OR**<sub>**1**</sub> and **OR**<sub>**2**</sub>). On the other hand, computation of the **worst case propagation delay** is not as simple as it appears. At first glance, it would appear that the **worst case** corresponds to path <i class="fa-circle-1">:circle-1:</i> and the delay is **5 t**<sub>**gate**</sub>. However, when analyzing the data dependencies, it becomes obvious that path <i class="fa-circle-1">:circle-1:</i> is **never exercised**. Path <i class="fa-circle-1">:circle-1:</i> is called a **false path**.

1. If **A = 1**, the **critical path** goes through **OR**<sub>**1**</sub> and **OR**<sub>**2**</sub>.
2. If **A = 0** and **B = 0**, the **critical path** is through **I**<sub>**1**</sub>, **OR**<sub>**1**</sub> and **OR**<sub>**2**</sub> (corresponding to a delay of **3 t**<sub>**gate**</sub>).
3. For the case when **A = 0** and **B = 1**, the **critical path** is through **I**<sub>**1**</sub>, **OR**<sub>**1**</sub>, **AND**<sub>**3**</sub> and **OR**<sub>**2**</sub>. OR<sub>1</sub> will output 0 to OR<sub>2</sub>, but 0 is a "non-controlling" value for an OR gate, so OR<sub>2</sub> needs to wait for the output from AND<sub>3</sub>.

In other words, for this simple (but contrived) **logic circuit**, the output does not even depend on inputs **C** and **D** (that is, there is redundancy). Therefore, the **propagation delay** is **4 t**<sub>**gate**</sub>. Given the **propagation** and **contamination delay**, the **minimum** and **maximum allowable skew** can be easily computed.

In this problem, we also see a very familiar concept — [**short circuiting**](https://wenbo-notes.gitbook.io/cs1010-notes/lec-tut-lab-exes/lecture/lec-04-conditionals#short-circuiting) (NUS CS1010 Knowledge comes back)! We now rephrase it using the concept of controlling value. A gate has a **controlling value** if one input alone can determine the output regardless of other inputs:

* **OR Gate**: Controlling value is 1. (If any input is 1, output is 1).
* **AND Gate**: Controlling value is 0. (If any input is 0, output is 0).

If one of the inputs of a gate is a noncontrolling value, the gate needs to wait for the other input.

{% hint style="danger" %}
The computation of the **worst-case propagation delay** for **combinational logic**, due to the existence of **false paths**, **cannot** be obtained by simply adding the **propagation delay** of individual **logic gates**. The **critical path** is strongly dependent on **circuit topology** and **data dependencies**.
{% endhint %}

</details>
