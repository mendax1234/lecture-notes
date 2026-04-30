# Lec 02 - CMOS Inverter

## Inverter

An inverter can be thought of as a NOT gate. It will negate whatever value from its input.

### Ideal Inverter

An ideal inverter has the following V<sub>in</sub>-V<sub>out</sub> graph (also known as Voltage Transfer Characteristic, or VTC)

<figure><img src="../.gitbook/assets/ideal-inverter-vin-vout.png" alt="" width="375"><figcaption></figcaption></figure>

From this graph, we can see that

1. When 0 <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>in</sub> <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>DD</sub> /2, V<sub>out</sub> = V<sub>DD</sub>, this means \[0, V<sub>DD</sub>/2] is Vin's **input low** range. Similarly, \[V<sub>DD</sub>/2, V<sub>DD</sub>] is Vin's **input high** range.
   1. [NM<sub>H</sub>](#user-content-fn-1)[^1]: V<sub>DD</sub> /2
   2. [NM<sub>L</sub>](#user-content-fn-2)[^2]: V<sub>DD</sub> /2
   3. [Switch threshold](#user-content-fn-3)[^3]: V<sub>DD</sub> /2
2. The meaning of **gain** is just the change rate of V<sub>out</sub> with respect to V<sub>in</sub>, which is $$d\frac{V_{out}}{V_{in}}$$

### Real Inverter

In the real world, the inverter cannot be that perfect. Its V<sub>in</sub>-V<sub>out</sub> graph is usually shown as follows

<figure><img src="../.gitbook/assets/real-inverter-vin-vout.png" alt="" width="375"><figcaption></figcaption></figure>

From this graph, we will see some very important conventions used in Electrical Engineering

1. In the V<sub>in</sub>-V<sub>out</sub> graph, we always treat x-axis as V<sub>in</sub> and y-axis as V<sub>out</sub>.
2. V<sub>OH</sub>: This is the **nominal (expected) output voltage** when the inverte**r output is logic HIGH**. This is usually around V<sub>DD</sub>.
3. V<sub>OL</sub>: This is the **nominal (expected) output voltage** when the inverter **output is logic LOW**. This is around 0V.
   1. V<sub>OH</sub> and V<sub>OL</sub> are **nominal values**, and they are **different from** the actual output, which we usually define to be just V<sub>out</sub>.
   2. Usually, V<sub>OH</sub>, V<sub>OL</sub>, together with V<sub>IL</sub> and V<sub>IH</sub> **are specified by the manufacturer** in the **logic family’s datasheet.**
4. V<sub>M</sub>: This is the **switching threshold**, where V<sub>in</sub> = V<sub>out</sub>.

#### Mapping between Analog and Digital Signals

As analog signals are **continuous** while digital signals are **discrete**, when we convert analog signals to digital signals, we need to define some "mappings". In this lecture, the following mapping is defined,

<figure><img src="../.gitbook/assets/real-inverter-vin-vout-vil-vih.png" alt="" width="375"><figcaption></figcaption></figure>

From this graph, we add on to the important conventions that will be used in Electrical Engineering

1. V<sub>IL</sub>: The **highest input voltage** that the gate will still recognize as a logic LOW.
   1. Thus, in this graph, the **input low** range is \[V<sub>OL</sub>, V<sub>IL</sub>]
2. V<sub>IH</sub>: The **lowest input voltage** that the gate will recognize as a logic HIGH.
   1. Thus, in this graph, the **input high** range is \[V<sub>IH</sub>, V<sub>OH</sub>]

By definition, V<sub>IH</sub> and V<sub>IL</sub> are the operational points of the inverter where $$\frac{dV_{\text{out}}}{dV_{\text{in}}}=-1$$.

#### Noise Margin

**Noise margin** is how much noise a signal can tolerate and still be **read** correctly as 0 or 1. So, in our notation, we can easily treat NM<sub>H</sub> as the length of **input high** range and NM<sub>L</sub> as the length of the **input low** range.

* NM<sub>H</sub> = V<sub>OH</sub> - V<sub>IH</sub>, or more precisely, V<sub>DD</sub> - V<sub>IH</sub>.
* NM<sub>L</sub> = V<sub>IL</sub> - V<sub>OL</sub>, or more precisely, just V<sub>IL</sub>.

<figure><img src="../.gitbook/assets/nmh-nml.png" alt="" width="375"><figcaption></figcaption></figure>

Prof. Annie also introduces the following analogy for NM<sub>H</sub> and NM<sub>L</sub>:

* NM<sub>H</sub> defines how much the V<sub>out</sub> can **drop** from the **nominal high voltage** (V<sub>OH</sub>) for it to be **read** correctly as logic HIGH by the following gate.&#x20;
* NML defines how much the V<sub>out</sub> can **go up** from the **nominal low voltage** (V<sub>OL</sub>) for it to be **read** correctly as logic LOW by the following gate.&#x20;

#### Fan-in and Fan-out

<figure><img src="../.gitbook/assets/fan-in-fan-out.png" alt="" width="563"><figcaption></figcaption></figure>

From the above image, we can clearly see that

1. **Fan-out** means how many **inputs** are driven by one output the number of gates that one output signal can **drive**.
2. **Fan-in** means how many **inputs** a gate has, it describes the number of signals feeding **into** one logic gate.

## CMOS Inverter

An inverter can be built using one PMOS and one NMOS shown as follows,

<figure><img src="../.gitbook/assets/cmos-inverter-example.png" alt="" width="375"><figcaption></figcaption></figure>

### Basic Operation

The basic operations of a CMOS inverter are the same as the inverter. Basically, it's just when input = 0, output = 1 and when input = 1, output = 0. But in this part, we will use the |VGS| thinking to decide the ON/OFF state of the PMOS and NMOS.

<figure><img src="../.gitbook/assets/cmos-inverter-with-gsd.png" alt="" width="372"><figcaption></figcaption></figure>

1. **Input = 0 -> Output = 1**: NMOS's |V<sub>GS</sub>| = 0 < |V<sub>TH</sub>|, NMOS is OFF. PMOS's |V<sub>GS</sub>| = V<sub>DD</sub> > |V<sub>TH</sub>|, PMOS  is ON. Thus, V<sub>out</sub> is connected to V<sub>DD</sub>, output is 1.
2. **Input = 1 -> Output = 0**: NMOS's |V<sub>GS</sub>| = V<sub>DD</sub> > |V<sub>TH</sub>|, NMOS is ON. PMOS's |V<sub>GS</sub>| = 0 < |V<sub>TH</sub>|, PMOS  is OFF. Thus, V<sub>out</sub> is connected to ground, output is 0.

<details>

<summary>Are V<sub>TH</sub> for NMOS and PMOS the same?</summary>

As you may notice, we are comparing the |V<sub>GS</sub>| with the **same** |V<sub>TH</sub>| in above. This is **not a typo!** In real world, we do **try to** make V<sub>TH</sub> of PMOS and NMOS the same. This is done by using the [**sizing technique**](https://wenbo-notes.gitbook.io/cg2027-notes/tut/tutorial-1#id-02.-on-resistance) we have introduced in Tutorial 1 to change the on resistance of the PMOS and NMOS.

</details>

### Switching Characteristics

As V<sub>in</sub> changes, the CMOS inverter goes through **five** different combinations of **regions of operation**. These combinations are indicated below:

1. 0 <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>in</sub> <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>TN</sub>: NMOS off, PMOS on.
2. V<sub>TN</sub> < V<sub>in</sub> < V<sub>inv</sub>: PMOS linear, NMOS saturation.
3. V<sub>in</sub> = V<sub>inv</sub>: Both PMOS and NMOS saturation
4. V<sub>inv</sub> < V<sub>in</sub> < V<sub>DD</sub> - |V<sub>TP</sub>|: NMOS linear, PMOS saturation
5. V<sub>DD</sub> - |V<sub>TP</sub>| < V<sub>in</sub> <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>DD</sub>: PMOS OFF, NMOS ON

where,

* V<sub>inv</sub> is the **switching voltage** (same as the V<sub>M</sub> we have introduced [above](lec-02-cmos-inverter.md#real-inverter))
* V<sub>TN</sub> is the **threshold voltage** of an NMOS.
* V<sub>TP</sub> is the **threshold voltage** of an PMOS.

To understand it more clearly, let's use the following example,

<figure><img src="../.gitbook/assets/cmos-inverter-switching-characteristic.jpg" alt=""><figcaption></figcaption></figure>

Suppose V<sub>TN</sub> = 0.3V, V<sub>TP</sub> = -0.3V, V<sub>DD</sub> = 1V and V<sub>inv</sub> = 0.5V

1. 0 <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>in</sub> <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>TN</sub> -> 0 <i class="fa-less-than-equal">:less-than-equal:</i> V<sub>in</sub> <i class="fa-less-than-equal">:less-than-equal:</i> 0.3: Let's take V<sub>in</sub> = 0.2V and assume Vout = 1V.
   1. NMOS: |V<sub>GS</sub>| < |V<sub>TN</sub>|, NMOS is OFF.
   2. PMOS: |V<sub>GS</sub>| <i class="fa-greater-than-equal">:greater-than-equal:</i> 0.7V > |V<sub>TP</sub>|, PMOS is ON. V<sub>GD</sub> = -0.8V < V<sub>TP</sub>, PMOS in **linear region**.
2. V<sub>TN</sub> < V<sub>in</sub> < V<sub>inv</sub> -> 0.4 < V<sub>in</sub> < 0.5: Let's take V<sub>in</sub> = 0.45V and assume V<sub>out</sub> drops a bit but is still high, is 0.9V.
   1. NMOS: |V<sub>GS</sub>| > |V<sub>TN</sub>|, NMOS is ON. V<sub>GD</sub> = -0.45V < V<sub>TN</sub>, NMOS operates in **saturation** **region**.
   2. PMOS: |V<sub>GS</sub>| > |V<sub>TP</sub>|, PMOS is ON. V<sub>GD</sub> = -0.45V < V<sub>TP</sub>, PMOS operates in **linear** **region.**
3. V<sub>in</sub> = V<sub>inv</sub>: Let's say V<sub>in</sub> = 0.5V and V<sub>out</sub> = 0.5V
   1. NMOS: |V<sub>GS</sub>| > |V<sub>TN</sub>|, NMOS is still on. V<sub>GD</sub> = 0 < V<sub>TN</sub>, NMOS operates in **saturation region**.
   2. PMOS: |V<sub>GS</sub>| > |V<sub>TP</sub>|, PMOS is still on. V<sub>GD</sub> = 0 > V<sub>TP</sub>, PMOS operates in **saturation region**.
4. V<sub>inv</sub> < V<sub>in</sub> < V<sub>DD</sub> - |V<sub>TP</sub>| -> 0.5 < V<sub>in</sub> < 0.6V. Let's take V<sub>in</sub> = 0.55V and assume V<sub>out</sub> is 0.2V.
   1. NMOS: |V<sub>GS</sub>| > |V<sub>TN</sub>|, NMOS is ON. V<sub>GD</sub> = 0.35V > V<sub>TN</sub>, NMOS operates in **linear region**.
   2. PMOS: |V<sub>GS</sub>| > |V<sub>TP</sub>|, PMOS is ON. V<sub>GD</sub> = 0.35V > V<sub>TP</sub>, PMOS operates in **saturation region**.
5. V<sub>DD</sub> - |V<sub>TP</sub>| < V<sub>in</sub> < V<sub>DD</sub> -> 0.6 < V<sub>in</sub> < 1. Let's take V<sub>in</sub> = 0.75V and assum V<sub>out</sub> is 0V.
   1. NMOS: |V<sub>GS</sub>| > |V<sub>TN</sub>|, NMOS is ON. V<sub>GD</sub> = 0.75V > V<sub>TN</sub>, NMOS operates in **linear region**.
   2. PMOS: |V<sub>GS</sub>| < |V<sub>TP</sub>|, PMOS is OFF.

{% hint style="success" %}
V<sub>in</sub> is the gate voltage V<sub>G</sub> here.
{% endhint %}

The above calculation is based on the following table we have introduced in Lec 01. As for the CMOS inverter case,

* For NMOS: V<sub>gsn</sub> = V<sub>in</sub>, V<sub>dsn</sub> = V<sub>out</sub>.
* For PMOS: V<sub>gsp</sub> = V<sub>in</sub> - V<sub>DD</sub>, V<sub>dsn</sub> = V<sub>in</sub> - V<sub>DD</sub>.

<figure><img src="../.gitbook/assets/cmos-region reference table.png" alt=""><figcaption></figcaption></figure>

To summarize the five regions of operations together into one image, we have the following VTC of CMOS inverter

<figure><img src="../.gitbook/assets/cmos-inverter-vtc.png" alt="" width="504"><figcaption></figcaption></figure>

Now, we can draw the [short-circuit current](#user-content-fn-4)[^4] (I<sub>inv</sub>) vs. Vin graph, and it looks like as follows,

<figure><img src="../.gitbook/assets/iinv-vin-graph.png" alt="" width="563"><figcaption></figcaption></figure>

From the graph above, we can see that the CMOS **seems to** only consume power when **switching**.

However, as I use "seems to" above, means that in reality, due to [**sub-threshold currents**](#user-content-fn-5)[^5], etc, there is still some power consumption when the CMOS inverter is in OFF state.

### Inverter with a Load

Normally, CMOS inverter will have parastic resistances and capacitances attached to the **output node**. Hence principle of operation of inverter alters a bit. This reflects in the appearnce of rise time (t<sub>r</sub>), fall time (t<sub>f</sub>), ..., we have introduced in [Lec 01](https://wenbo-notes.gitbook.io/cg2027-notes/tut/tutorial-1#id-01.-delay-calculation).

<figure><img src="../.gitbook/assets/inverter-with-load-with-timing-diagram.png" alt=""><figcaption></figcaption></figure>

These time delays appear because **charging** and **discharging** the capacitor attached to the output node takes time! (CG1111A again!)

{% columns %}
{% column %}
#### Charging the load

<figure><img src="../.gitbook/assets/inverter-with-load-charge.png" alt="" width="371"><figcaption></figcaption></figure>

When V<sub>in</sub> = 0V, the PMOS will turn on the current will flow from V<sub>DD</sub>, through the PMOS device (it has on resistance!) and finally through the capacitor to charge it until the capacitor is fully charged.

{% hint style="success" %}
That's why PMOS device is called **pull-up device**.
{% endhint %}


{% endcolumn %}

{% column %}
#### Discharging the load

<figure><img src="../.gitbook/assets/inverter-with-load-discharge.png" alt="" width="371"><figcaption></figcaption></figure>

After the capacitor is fully charged, it has the polarity shown as above. Now, if V<sub>in</sub> = 1, our PMOS will be turned off and NMOS will be turned ON, forming a circuit so that the current can flow from the capacitor, through the NMOS device, and lastly to the ground until the capacitor is finally discharged.

{% hint style="success" %}
That's why NMOS device is called **pull-down device**.
{% endhint %}


{% endcolumn %}
{% endcolumns %}

#### Transient Response

As it takes time to charge and discharge the capacitor, we can do the transient analysis to get a formula for calculating t<sub>pLH</sub> (when charging) and t<sub>pHL</sub> (when discharging).

<figure><img src="../.gitbook/assets/inverter-with-load-tplh-tphl.png" alt="" width="563"><figcaption></figcaption></figure>

For an RC circuit, the capacitor voltage at time $$t$$ is given by

$$
V(t)=V_{\text{DD}}\cdot e^{-t/RC}
$$

Recall that when we are doing the t<sub>pLH</sub> and t<sub>pHL</sub> calculation, we are only interested in the 50% point. So

* When charging, t<sub>pLH</sub> is the time taken for Voltage across the capacitor to go from 0 -> V<sub>DD</sub>/2.
* When discharging, t<sub>pHL</sub> is the time taken for Voltage across the capacitor to go from V<sub>DD</sub>/2 -> 0.

Thus, we can plug V(t) = V<sub>DD</sub>/2 into our formula and solve for $$t$$

$$
\begin{align*}\frac{V_{\text{DD}}}{2}&=V_{\text{DD}}\cdot e^{-t/RC}\\
t&=\ln2\cdot R\cdot C\\t&\approx0.69\cdot R\cdot C\end{align*}
$$

where $$R$$ is the **on resistance** of the PMOS/NMOS device, and $$C$$ is the capacitance of the capacitor.

{% hint style="success" %}
Hence, a **fast gate** is built either by keeping the output capacitance **small** or by decreasing the on-resistance of the transistor.
{% endhint %}

There is also a second approach to know the propagation delay if we don't know the **on-resistance**, it is introduced [later](lec-02-cmos-inverter.md#cmos-inverter-propagation-delay).

## Power Dissipation

As we have seen above, when the CMOS is switching, the **short circuit current** will consume some power. Also, we have mentioned about the **leakage current**, which will also consume some power. Lastly, as we have introduced the capacitors above, charging and discharging the capacitors will also consume some power, this is called [**dynamic power consumption**](lec-02-cmos-inverter.md#dynamic-power-consumption).

### Dynamic Power Consumption

We have already seen the image when we are charging the capacitor,

<figure><img src="../.gitbook/assets/dynamic-power-consumption-charging.png" alt="" width="563"><figcaption></figcaption></figure>

The **total** energy/transition is $$C_L\cdot V_{\text{DD}}^2$$, and it can be calculated by integrating the following

$$
E_{\text{VDD}}=\int_0^\infty i_{\text{VDD}}(t)\cdot V_{\text{DD}}~dt
$$

Since $$V_{\text{DD}}$$ is constant, move it outside the integral:

$$
\begin{align*}
E_{\text{VDD}} &= V_{\text{DD}} \int_0^\infty i_{\text{VDD}}(t)~dt
\end{align*}
$$

For a capacitive load $$C_L$$, use $$i_{\text{VDD}}(t) = C_L \frac{dv_{\text{out}}(t)}{dt}$$ (during charging, where $$v_{\text{out}}$$ is the output voltage):

$$
\begin{align*}
E_{\text{VDD}} &= V_{\text{DD}} \int_0^\infty C_L \frac{dv_{\text{out}}(t)}{dt}~dt
\end{align*}
$$

Move $$C_L$$ outside and change variable to $$v = v_{\text{out}}$$ (substitute $$dt = dv / (dv/dt)$$):

$$
\begin{align*}
E_{\text{VDD}} &= V_{\text{DD}}\cdot C_L \int_0^{V_{\text{DD}}}1~dv
\end{align*}
$$

Evaluate the integral (limits from 0 to $$V_{\text{DD}}$$ as $$v_{\text{out}}$$ charges fully):

$$
\begin{align*}
E_{\text{VDD}} &= V_{\text{DD}} \cdot C_L \cdot V_{\text{DD}} = C_L V_{\text{DD}}^2
\end{align*}
$$

Its power is energy/transition times frequency f, which is $$C_L\cdot V_{\text{DD}}^2\cdot f$$. However, we can see the in this circuit, the capacitor will also take some power consumption, and it can be calculated by integrating the follwoing, which will give us $$\frac{1}{2}\cdot C_L\cdot V_{\text{DD}}^2$$,

$$
E_{C_L}=\int_0^\infty i_{\text{VDD}}(t)\cdot v_{\text{out}}~dt
$$

Similarly as above, for a capacitive load $$C_L$$, use $$i_{\text{VDD}}(t) = C_L \frac{dv_{\text{out}}(t)}{dt}$$ (during charging, where $$v_{\text{out}}$$ is the output voltage):

$$
\begin{align*}
E_{C_L} &= \int_0^\infty C_L \frac{dv_{\text{out}}(t)}{dt} \cdot v_{\text{out}}(t)~dt
\end{align*}
$$

Move $$C_L$$ outside the integral:

$$
\begin{align*}
E_{C_L} &= C_L \int_0^\infty v_{\text{out}}(t) \cdot \frac{dv_{\text{out}}(t)}{dt}~dt
\end{align*}
$$

Substitute $$v = v_{\text{out}}$$, so $$dv = \frac{dv_{\text{out}}}{dt}dt$$, and change the limits of integration from $$t: 0 \to \infty$$ to $$v: 0 \to V_{\text{DD}}$$ (as $$v_{\text{out}}$$ charges from 0 to $$V_{\text{DD}}$$):

$$
\begin{align*}
E_{C_L} &= C_L \int_0^{V_{\text{DD}}} v~dv
\end{align*}
$$

Evaluate the integral:

$$
\begin{align*}
\int_0^{V_{\text{DD}}} v~dv &= \left[ \frac{1}{2} v^2 \right]_0^{V{\text{DD}}} = \frac{1}{2} V_{\text{DD}}^2 - 0 = \frac{1}{2} V_{\text{DD}}^2.
\end{align*}
$$

Thus, the final result is:

$$
\begin{align*}
E_{C_L} &= C_L \cdot \frac{1}{2} V_{\text{DD}}^2 = \frac{1}{2} C_L V_{\text{DD}}^2
\end{align*}
$$

But this is only **half** of the total energy consumption. Where does the rest **half** go? If you think about it, it won't be hard to find out that the other half is dissipated as **heat** on the PMOS's resistor.

{% hint style="warning" %}
The above analysis is based on the PMOS! In other words, when we are **charging**! When we are **discharging**, the **energy consumption from the power supply** is **0J**! And all the energy stored in the capacitor ($$\frac{1}{2}\cdot C_L\cdot V_{\text{DD}}^2$$) will dissipate as heat on the NMOS's resistor.
{% endhint %}

#### Modification for Circuits with Reduced Swing

As we have seen above, we can exploit **reduced swing** to lower the power. And the circuit after modification is shown below,

<figure><img src="../.gitbook/assets/modification-for-circuits-using-reduced-swings.png" alt="" width="563"><figcaption></figcaption></figure>

And now the energy per transition will be $$E_{0 \to 1} = C_L \cdot V_{\text{dd}} \cdot (V_{\text{dd}} - V_t)$$ and the energy for the capacitor will be $$\frac{1}{2}\cdot C_L\cdot (V_{\text{DD}}-V_t)^2$$.

{% hint style="success" %}
We can just change the upper limit from $$V_{\text{DD}}$$ to $$V_{\text{DD}}-V_t$$ to get the above result. The integration processes are exactly the same.
{% endhint %}

#### Node Transition Activity and Power

Now, let's consider switching a CMOS gate for $$N$$ clock cycles

$$
E_N = C_L \cdot V_{\text{dd}}^2 \cdot n(N)
$$

where $$E_N$$ is the energy consumed for $$N$$ clock cycles and $$n(N)$$ is the number of $$0\to1$$ transitions in $$N$$ clock cycles. Thus, we can have the average power $$P_{\text{avg}}$$ as follows,

$$
\begin{align*}
P_{\text{avg}} &= \lim_{N \to \infty} \frac{E_N}{N} \cdot f_{\text{clk}} = \left( \lim_{N \to \infty} \frac{n(N)}{N} \right) \cdot C_L \cdot V_{\text{dd}}^2 \cdot f_{\text{clk}}
\end{align*}
$$

We can further define the **activity factor** $$\alpha_{0\to1}$$ as follows,

$$
\begin{align*}
\alpha_{0 \to 1} &= \lim_{N \to \infty} \frac{n(N)}{N}
\end{align*}
$$

And now, we can rewrite our average power $$P_{\text{avg}}$$ as follows,

$$
\begin{align*}
P_{\text{avg}} &= \alpha_{0 \to 1} \cdot C_L \cdot V_{\text{dd}}^2 \cdot f_{\text{clk}}
\end{align*}
$$

#### CMOS Inverter Propagation Delay

In the [#transient-response](lec-02-cmos-inverter.md#transient-response "mention") analysis, we have already seen how to calculate the propagation delay of a CMOS inverter when we know the NMOS/PMOS **on-resistance** as well as the capacitance $$C_L$$. Now, we introduce the second method to know the propagation delay if we don't know the **on-resistance**, but know the **average current** I<sub>av</sub> in the circuit.

Starting with $$i = C \frac{dv}{dt}$$, move $$dt$$ to the L.H.S and integrate both sides:

$$
\begin{align*}
i ~ dt &= C ~ dv
\end{align*}
$$

Integrate from initial to final conditions, where $$v$$ goes from $$v_1$$ to $$v_2$$ and time $$t_p$$ is the propagation delay:

$$
\begin{align*}
\int_0^{t_p} i(t)~dt &= \int_{v_1}^{v_2} C~dv
\end{align*}
$$

Since $$C = C_L$$ is constant, factor it out:

$$
\begin{align*}
C_L \int_0^{t_p} i(t)~dt &= C_L \int_{v_1}^{v_2}1~dv
\end{align*}
$$

The right-hand side integrates to the voltage difference:

$$
\begin{align*}
C_L \int_0^{t_p} i(t)~dt &= C_L (v_2 - v_1)
\end{align*}
$$

The left-hand side represents the charge transferred, and since $$I_{\text{av}} = \frac{1}{t_p} \int_0^{t_p} i(t)~dt$$, substitute $$I_{\text{av}} \cdot t_p$$:

$$
\begin{align*}
I_{\text{av}} \cdot t_p &= C_L (v_2 - v_1)
\end{align*}
$$

Solve for $$t_p$$:

$$
\begin{align*}
t_p &= \frac{C_L (v_2 - v_1)}{I_{\text{av}}}
\end{align*}
$$

For the high-to-low propagation delay $$t_{\text{PHL}}$$, $$v_2 - v_1 = V_{\text{swing}}$$ (full swing from $$V_{\text{DD}}$$ to 0, but noting $$t_p$$ is 50% of full swing), so we have:

$$
t_{\text{PHL}} = \frac{C_L V_{\text{swing}}/2}{I_{\text{av}}}
$$

## Load Capacitance / Resistance

### Load Capacitance

When a CMOS gate **drives** another CMOS gate, the driver "sees" a [**capacitive load**](#user-content-fn-6)[^6]. The load capacitance consists of

1. **gate capacitance** of the load from two MOSFETS on the right (C<sub>g</sub>)
2. **junction capacitance** from common **drain** of the driver inverter on the left (C<sub>j</sub>), and
3. the **parastic routing capacitance** from the metal line used for connection (C<sub>w</sub>)

<figure><img src="../.gitbook/assets/load-capacitance-definition.png" alt="" width="375"><figcaption></figcaption></figure>

In other words, the **load capacitcance** is

$$
C_L=C_g+C_j+C_w
$$

#### Transistor

{% stepper %}
{% step %}
#### Gate Capacitance

Gate capacitances estimation in different regions is below. However many times **a constant value** is used.

<figure><img src="../.gitbook/assets/load-capacitance-estimation-transistor.png" alt="" width="362"><figcaption></figcaption></figure>

| Parameter  | Off                                   | Linear                                 | Saturation                              |
| ---------- | ------------------------------------- | -------------------------------------- | --------------------------------------- |
| $$C_{gb}$$ | $$\frac{\epsilon_{sio_2} A}{t_{ox}}$$ | 0                                      | 0                                       |
| $$C_{gs}$$ | 0                                     | $$\frac{\epsilon_{sio_2} A}{2t_{ox}}$$ | $$\frac{2\epsilon_{sio_2} A}{3t_{ox}}$$ |
| $$C_{gd}$$ | 0                                     | $$\frac{\epsilon_{sio_2} A}{2t_{ox}}$$ | 0                                       |

In this table, $$\epsilon_{sio_2}=\epsilon_o\cdot\epsilon_v$$, and $$A$$ is the area, which is the same as $$W\cdot L$$.
{% endstep %}

{% step %}
#### Wire Capacitance

$$
C_{\text{wire}}=C_{pp}+C_{\text{fringe}}
$$

$$C_{pp}$$ is the same as $$C_{\text{area}}$$, which can be calculated using

$$
C_{\text{area}}=C_{\text{per unit area}}\cdot W\cdot L \tag{1}
$$

where, $$C_{\text{per unit area}}=\frac{\epsilon}{t}$$, and $$\epsilon$$ is the **electron mobility** and $$t$$ is the thickness. However, the $$C_{\text{per unit area}}$$ will usually be given.

{% hint style="info" %}
The Eq. (1) applies to **any capacitance** calculation.
{% endhint %}
{% endstep %}
{% endstepper %}

### Load Resistance

To calculate the resistance, we can use the following formula

$$
R=\frac{\rho\cdot L}{H\cdot W}
$$

where we treat $$\frac{\rho}{H}$$ as a whole and name it as **sheet resistance** with the unit ( $$\Omega/\square$$) and $$\frac{L}{W}$$ has the unit of $$\square$$ and they **must** be converted to the same unit before the division!

[^1]: For V<sub>out</sub> to be **high**, the acceptable range of V<sub>in</sub>

[^2]: For V<sub>out</sub> to be **low**, the acceptable range of V<sub>in</sub>

[^3]: The value of V<sub>in</sub> that causes V<sub>out</sub> to change from low to high or high to low.

[^4]: the current that flows directly from V<sub>DD</sub> to Ground

[^5]: one of the main sources of **leakage current**, which is a tiny amount of current that leaks through the device when it is idle.

[^6]: This is the $$C_L$$ we used in all our previous sections.
