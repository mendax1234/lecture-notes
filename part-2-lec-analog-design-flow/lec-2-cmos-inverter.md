# Lec 2 - CMOS Inverter

## CMOS Inverter

This par requires a good understanding of what's been discussed in [CG2027 Lec 02](https://app.gitbook.com/s/6nPr3SObC3azazbFhfgF/lec/lec-02-cmos-inverter).

### Layout

The circuit symbol and layout of a CMOS Inverter can be shown as follows.

<figure><img src="../.gitbook/assets/CMOS-Inverter-symbol-layout.png" alt="" width="563"><figcaption></figcaption></figure>

Note that the voltage at the output $$V_{\text{out}}$$ is always **smaller** than $$V_{\text{DD}}$$ and **bigger** than $$0$$, a.k.a, $$0<V_{\text{out}}<V_{\text{DD}}$$. Remember the **assumptions** we made in [NMOS](https://wenbo-notes.gitbook.io/ee4415-icd-notes/part-2-lec-analog-design-flow/lec-1-mosfet-and-cmos-process#assumptions) and [PMOS](https://wenbo-notes.gitbook.io/ee4415-icd-notes/part-2-lec-analog-design-flow/lec-1-mosfet-and-cmos-process#assumptions-1) source and drain in Lec 01? Basically they are

* In NMOS, the voltage at source is **lower** than the voltage at the drain.
* In PMOS, the voltage at source is **higher** than the voltage at the drain.

So, in the CMOS Invertor circuit symbol, it is obvious to figure out the source and drain position and this is shown in the figure above.

### Properties

In the CMOS inverter, we have the following properties

{% stepper %}
{% step %}
#### Full output swing

We have full rail-to-rail output swing.
{% endstep %}

{% step %}
#### Low-impedance path

There is always a **low-impedance path** which can be either

1. from the $$V_{\text{DD}}$$ to the $$V_{\text{out}}$$, or
2. from the $$V_{\text{out}}$$ to the $$\text{GND}$$
{% endstep %}

{% step %}
#### Direct path

There is **no** direct path between $$V_{\text{DD}}$$ and $$\text{GND}$$ in **steady state**. Thus, there is **no static power consumption**.

{% hint style="warning" %}
#### Steady State

In the steady state, the inputs and outputs are held steady.
{% endhint %}
{% endstep %}
{% endstepper %}

## Static Analysis

In this part, we will mainly see if we change the gate voltage $$V_{\text{gate}}$$ or the input voltage $$V_{\text{in}}$$ (they are exactly the same in this case), what will happen to the CMOS Inverter.

### Voltage Transfer Characteristic

The voltage transfer characteristic is simply a diagram denoting the relationship between $$V_{\text{in}}$$ and $$V_{\text{out}}$$. One example is shown below.

<figure><img src="../.gitbook/assets/vtc-example.png" alt=""><figcaption></figcaption></figure>

1. When $$V_{\text{in}}$$ is low, $$V_{\text{out}}$$ is high. In this case
   1. For the NMOS, it is likely that $$V_{\text{GS}}<V_{\text{TN}}$$. Thus, NMOS is **Off**.
   2. For the PMOS, it is likely that $$V_{\text{GS}}<V_{\text{TP}}$$  (L.H.S. is around negative $$V_{\text{DD}}$$ while R.H.S is a small negative number) and $$V_{\text{DS}}>V_{\text{GS}}-V_{\text{TP}}$$ (L.H.S is around 0 while R.H.S is confirm negative). Thus, PMOS is in **Linear.**
2. When $$V_{\text{in}}$$ increases a bit but $$V_{\text{out}}$$ is still high, in this case
   1. For the NMOS, it is likely that $$V_{\text{GS}}>V_{\text{TN}}$$ and $$V_{\text{DS}}>V_{\text{GS}}-V_{\text{TN}}$$ (L.H.S is around $$V_{\text{DD}}$$ while R.H.S is around 0). Thus, NMOS is in **Saturation**.
   2. For the PMOS, it is likely that $$V_{\text{GS}}<V_{\text{TP}}$$ (L.H.S. is still around negative $$V_{\text{DD}}$$ while R.H.S is a small negative number) but $$V_{\text{DS}}>V_{\text{GS}}-V_{\text{TP}}$$ (L.H.S is around 0 while R.H.S is negative). Thus PMOS is in **Linear**.
3. When $$V_{\text{in}}=V_{\text{out}}=\frac{V_{\text{DD}}}{2}$$, in this case
   1. For the NMOS, it is likely that it will be in **Saturation**.
   2. For the PMOS, same likelihood that $$V_{\text{GS}}<V_{\text{TP}}$$ but $$V_{\text{DS}}<V_{\text{GS}}-V_{\text{TP}}$$ (as $$V_{\text{DS}}=V_{\text{GS}}$$ now, L.H.S is definitely **smaller** than R.H.S). Thus PMOS will be in **Saturation**.
4. When $$V_{\text{in}}$$ is a little higher, $$V_{\text{out}}$$ is low. In this case
   1. For the NMOS, it is definitely on. $$V_{\text{DS}}<V_{\text{GS}}-V_{\text{TN}}$$ (L.H.S is around 0 while R.H.S is definitely a **positive** number). Thus NMOS is in **Linear**.
   2. For the PMOS, it is likely that $$V_{\text{GS}}<V_{\text{TP}}$$ and $$V_{\text{DS}}<V_{\text{GS}}-V_{\text{TP}}$$ (L.H.S is around negative $$V_{\text{DD}}$$ while R.H.S is a less negative number). Thus PMOS is in **Saturation**.
5. When $$V_{\text{in}}$$ is a little higher, $$V_{\text{out}}$$ is low. In this case
   1. For the NMOS, it is likely that it will be in **Saturation**.
   2. For the PMOS, it is likely that $$V_{\text{GS}}>V_{\text{TP}}$$ (both negative here). Thus, PMOS is in **Off**.

In addition, the region between step 2 to step 4 is called **high-gain region** because the gain[^1] of the curve is **quite high** in terms of its magnitude.

<details>

<summary>Example of CMOS Inverter VTC</summary>

{% hint style="danger" %}
Read the termologies below before opening this example.
{% endhint %}

Using SPICE to generate an Inverter VTC for the circuit below,

<figure><img src="../.gitbook/assets/cmos-inverter-vtc.png" alt=""><figcaption></figcaption></figure>

we may get the following combined diagram of VTC and gain vs. $$V_{\text{in}}$$.

<figure><img src="../.gitbook/assets/vtc-gain-combined.png" alt=""><figcaption></figcaption></figure>

</details>

### Terminologies

It is good to utilize the VTC to review the following very important terminologies

{% stepper %}
{% step %}
#### $$V_{\text{OH}}$$ and $$V_{\text{OL}}$$

$$V_{\text{OH}}$$ and $$V_{\text{OL}}$$ are the output voltages when the **ideal input** low and high are applied to the CMOS Inverter respectively.

{% hint style="warning" %}
Usually, as the ideal input low and high are 0 and $$V_{\text{DD}}$$ respectively, $$V_{\text{OH}}$$ and $$V_{\text{OL}}$$ are thus $$V_{\text{DD}}$$ and 0.
{% endhint %}
{% endstep %}

{% step %}
#### Switching Threshold $$V_M$$

The switching threshold $$V_M$$ is when the input voltage is equal to the output voltage ($$V_{\text{in}}=V_{\text{out}}$$). The value can be found easily by drawing a line $$y=x$$ in the VTC graph and finding the x/y-coordinate of the intersection point.

If we want to find the value of $$V_M$$ analytically, we can do so by equating the currents through the transistors.

> WHy we can equate the current? Is it because the voltage drop is the same and we usually size the PMOS and NMOS to have the same resistance?

This will give us

$$
V_{\text{M}} =
\frac{
\left(V_{\text{THn}} + \frac{V_{\text{DSATn}}}{2}\right)
+ r\left(V_{\text{DD}} - |V_{\text{THp}}| - \frac{|V_{\text{DSATp}}|}{2}\right)
}{1 + r}
$$

where the ratio $$r$$ compares the **relative driving strengths** of the PMOS and NMOS transistors.

$$
r =
\frac{k_{\text{p}} V_{\text{DSATp}}}{k_{\text{n}} V_{\text{DSATn}}}
=
\frac{k'_{\text{p}} (W/L)_{\text{p}} V_{\text{DSATp}}}
{k'_{\text{n}} (W/L)_{\text{n}} V_{\text{DSATn}}}
$$

The intuition of the ratio $$r$$ is that: $$r$$ is bigger -> PMOS is stronger -> $$V_M$$ is bigger.

{% hint style="warning" %}
The ratio $$W/L$$ is the **size** of NMOS/PMOS.
{% endhint %}

<details>

<summary>Impact of <span class="math">r</span> on <span class="math">V_M</span></summary>

Compare the switching threshold ($$V_M$$) of inverters with different $$\frac{(W/L)_p}{(W/L)n}$$.

<figure><img src="../.gitbook/assets/impact-of-r-on-vm.png" alt=""><figcaption></figcaption></figure>

The SPICE diagram we get is as follows.

<figure><img src="../.gitbook/assets/spice-impact-r-on-vm.png" alt=""><figcaption></figcaption></figure>

From this diagram, we can clearly see that it matches with our intuition on $$r$$.

{% hint style="warning" %}
#### PMOS is generally weaker than NMOS

In the blue curve, when the size of NMOS is the same as the size of PMOS, the threshold voltage is slightly lower than $$V_{\text{DD}}/2$$, this is because when the size of PMOS and NMOS is the same, PMOS is **weaker** than NMOS. This is because of the **mobility** of holes in p-type material is **smaller** than the mobility of electrons in the n-type material.
{% endhint %}

</details>
{% endstep %}

{% step %}
#### $$V_{\text{IH}}$$ and $$V_{\text{IL}}$$

$$V_{\text{IH}}$$ and $$V_{\text{IL}}$$ are the **minimum** and **maximum** input voltage $$V_{\text{in}}$$ that will cause the output voltage $$V_{\text{out}}$$ to be **low** and **high** respectively.

{% hint style="warning" %}
$$V_{\text{IH}}$$ and $$V_{\text{IL}}$$ are the points where $$d\frac{V_{\text{out}}}{V_{\text{in}}}=-1$$ on the VTC.
{% endhint %}
{% endstep %}

{% step %}
#### Noise Margin $$\text{NM}_H$$ and $$\text{NM}_L$$

* $$\text{NM}_H$$ defines how much the $$V_{\text{out}}$$ can **drop** from the **nominal high voltage** ($$V_{\text{OH}}$$) for it to be **read** correctly as logic HIGH by the following gate.
* $$\text{NM}_L$$ defines how much the $$V_{\text{out}}$$ can **go up** from the **nominal low voltage** ($$V_{\text{OL}}$$) for it to be **read** correctly as logic LOW by the following gate.

Mathematically, $$\text{NM}_H=V_{\text{OH}}-V_{\text{IH}}$$ and $$\text{NM}_L=V_{\text{IL}}-V_{\text{OL}}$$.

{% hint style="success" %}
We want the noise margin to be **bigger**. Thus, the **gain** at the high-gain region should be as big as possible.
{% endhint %}

<details>

<summary>Gain of the inverter</summary>

Gain is nothing but the slope and it can be found easily in the VTC. Mathematically, it is defined as $$d\frac{V_{\text{out}}}{V_{\text{in}}}$$. Thus, an example of the gain vs. $$V_{\text{in}}$$ diagram can be shown as follows.

<figure><img src="../.gitbook/assets/gain-vin-diagram.png" alt="" width="563"><figcaption></figcaption></figure>

</details>
{% endstep %}
{% endstepper %}

## Dynamic Analysis

In this part, we will mainly see what factors will affect the propagation delay of the CMOS Inverter.

### Propagation Delay

In the CMOS Inverter, the **propagation delay** is caused by the charge and discharge of the load capacitance.

<figure><img src="../.gitbook/assets/charge-discharge.png" alt=""><figcaption></figcaption></figure>

#### Rise/Fall Time

This will also give rise to the definition of **rise time**, **fall time**, high-to-low propagation delay and low-to-high propagation delay.

<figure><img src="../.gitbook/assets/delay-definition.png" alt=""><figcaption></figcaption></figure>

The overall propagation delay is the **average** of the $$t_{\text{pHL}}$$ and $$t_{\text{pLH}}$$.

{% hint style="warning" %}
Rise time and fall time are both used on $$V_{\text{out}}$$ and sometimes might be from 5% -> 95% or 20% -> 80% depending on the specific context.
{% endhint %}

#### Propagation Delay Calculation

To calculate the propagation delay $$t_p$$, as we have seen above, we need to know what $$t_{\text{pHL}}$$ and $$t_{\text{pLH}}$$ are. Also, we've seen that they are measured when $$V_{\text{out}}$$ dropped from $$V_{\text{DD}}$$ to $$\frac{V_{\text{DD}}}{2}$$ or rose from 0 to $$\frac{V_{\text{DD}}}{2}$$.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### Equivalent On Resistance

As either NMOS or PMOS will be turned on stably when $$V_{\text{in}}$$ is equal to $$V_{\text{DD}}$$ or 0, it is good to know what's the **on resistance** of the NMOS and PMOS respectively.

{% hint style="warning" %}
In [NMOS](https://wenbo-notes.gitbook.io/ee4415-icd-notes/part-2-lec-analog-design-flow/lec-1-mosfet-and-cmos-process#linear-and-small), we assume that $$V_{\text{GS}}=V_{\text{DD}}$$ while in [PMOS](https://wenbo-notes.gitbook.io/ee4415-icd-notes/part-2-lec-analog-design-flow/lec-1-mosfet-and-cmos-process#linear-and-small-1), we assume that $$V_{\text{GS}}=-V_{\text{DD}}$$. In both cases, the formula to calcualte the on resistance is $$R_{\text{eq}}=V_{\text{DS}}\div I_{\text{DS}}$$.
{% endhint %}

Here, we will use NMOS as an example and PMOS will be left as an exercise for the interested readers. We will introduce two methods to get the on resistance.

{% hint style="warning" %}
In either one of the two methods, it is **not important** to understand the maths calculation, but **important** to know the final **approximation results**.
{% endhint %}

{% stepper %}
{% step %}
#### Integration

By integration, we have

$$
R_{\text{eq}} = -\frac{2}{V_{\text{DD}}} 
\int_{V_{\text{DD}}}^{V_{\text{DD}}/2} 
\frac{v}{I_{\text{Dsat}}(1+\lambda v)} \, dv 
\approx \frac{3V_{\text{DD}}}{4I_{\text{Dsat}}} 
\left(1 - \frac{7}{9}\lambda V_{\text{DD}}\right)
$$
{% endstep %}

{% step %}
#### Two endpoints

The method of two endpoints basically is to calculate the two resistance when $$V_{\text{DS}}$$ is $$V_{\text{DD}}$$ and $$V_{\text{DD}}/2$$. Then the final on resistance is just the average of the sum of these two resistances.

<figure><img src="../.gitbook/assets/two-endpoint-method.png" alt=""><figcaption></figcaption></figure>

The on resistance at the two endpoints is just the **slope**, which is where we use the ohm's law.

{% hint style="danger" %}
We assume that when $$V_{\text{DS}}$$ is $$V_{\text{DD}}$$ and $$V_{\text{DD}}/2$$, the NMOS is in **saturation region**.
{% endhint %}

Thus, the final formula we can get is as below.

$$
R_{\text{eq}} = \frac{1}{2} \left(
\frac{V_{\text{DD}}}{I_{\text{Dsat}}(1+\lambda V_{\text{DD}})} 
+ 
\frac{V_{\text{DD}}/2}{I_{\text{Dsat}}(1+\lambda V_{\text{DD}}/2)}
\right)
\approx \frac{3V_{\text{DD}}}{4I_{\text{Dsat}}} 
\left(1 - \frac{5}{6}\lambda V_{\text{DD}}\right)
$$

{% hint style="warning" %}
The ohm's law applies in **saturation region** as well. It's just that in the saturation region, the resistance of the MOSFET will change, not like in the linear region, the resistance of the MOSFET remains uniform.
{% endhint %}
{% endstep %}
{% endstepper %}

### Inverter Delay

This is just an example of calculating the inverter delay. Assume that the process is $$0.25\mu \text{m}$$, $$V_{\text{DD}}=2.5\text{V}$$, $$(W/L)_n=1.5$$, $$(W/L)_p=4.5$$ and $$C_{L}=6\text{fF}$$ and we are using the unified device model shown below. What is the propagation delay of this inverter?

<figure><img src="../.gitbook/assets/unified-model-0.25um.png" alt=""><figcaption></figcaption></figure>

**Sol**: The idea is to separate into two parts first: NMOS and PMOS. And then find the $$I_{\text{DSAT}}$$ and thus $$R_{\text{eq}}$$ for each of the two part. Then use the $$\text{RC}$$ equation to get the $$t_{\text{pHL}}$$ and $$t_{\text{pLH}}$$. Finally we can get the $$t_p$$.

<figure><img src="../.gitbook/assets/inverter-delay-example.png" alt=""><figcaption></figcaption></figure>

#### Design for Speed

From the equation we derive in [#propagation-delay-calculation](lec-2-cmos-inverter.md#propagation-delay-calculation "mention"), we can see that, in order to reduce the propagation delay, we can start from the following two aspects:

1. Reduce $$C_L$$
2. Increase $$W/L$$ ratio of the transistor: This is equivalent to reducing the **resistance** of the MOSFET as the size of the MOSFET is **inverse proportional** to the **resistance** of the MOSFET.

## Inverter Chain

Suppose we have the interter chain with a given $$C_L$$ as below. How many stages are needed to **minimize** the delay and how do we size the inverters?

<figure><img src="../.gitbook/assets/inverter-chain-opening-question.png" alt=""><figcaption></figcaption></figure>

Intuitively, we might think less gates means less propagation delay. However, we may soon find out this is **not true** in this case!

#### Inverter with a Load

In a real CMOS inverter, except for the external load $$C_L$$, it also has **internal load** which composes of two parts:

1. the gate capacitance $$C_{\text{gin}}$$, and
2. the intrinsic capacitance $$C_{\text{int}}$$

The location of these capacitance is shown clearly in the following figure.

<figure><img src="../.gitbook/assets/inverter-load-location.png" alt="" width="563"><figcaption></figcaption></figure>

For the gate capacitance, it is **proportional** to the size of the MOSFET. In other words, **bigger** MOSFET size means

1. bigger drive strength
2. lower resistance
3. bigger capacitance

The reference capacitance $$C_{\text{unit}}$$ is the **gate capacitance** of a unit-sized NMOS.

<details>

<summary>Defining <span class="math">C_{\text{unit}}</span> with exact values</summary>

What if in the Cadence tool, we have the exact value of $$W$$ and $$L$$ of our MOSFET, how are we going to use the $$C_{\text{unit}}$$ to get the gate capacitance $$C_{\text{gate}}$$ of the MOSFET?

***

The answer is that we don't use $$C_{\text{unit}}$$ anymore. Instead, we use the following formula:

<p align="center"><span class="math">C_{\text{gate}} = W \cdot L \cdot C_{\text{ox}}</span></p>

$$W$$, $$L$$ and $$C_{\text{ox}}$$ will be given and we just need to plug in the numbers accordingly to get the $$C_{\text{gate}}$$ of the MOSFET.

</details>

The real inverter delay can thus be calculated using the following formula.

$$
t_p = 0.69 R_{\text{eq}} (C_{\text{int}} + C_L) 
= 0.69 R_{\text{eq}} C_{\text{int}} + 0.69 R_{\text{eq}} C_L 
= 0.69 R_{\text{eq}} C_{\text{int}} \left( 1 + \frac{C_L}{C_{\text{int}}} \right)
$$

{% hint style="warning" %}
$$C_{\text{int}}$$ is treated as a **constant** and it is completely determined by the process!
{% endhint %}

The plot of $$t_p-C_L$$ is shown below for reference.

<figure><img src="../.gitbook/assets/tp-cl-plot.png" alt="" width="383"><figcaption></figcaption></figure>

<details>

<summary>INV1x, INV2x, INV4x, what are they?</summary>

In the CMOS inverter, we usually size the PMOS to the **twice** larger than the NMOS so that we can have roughly the same $$t_{\text{pLH}}$$ and $$t_{\text{pHL}}$$.

{% hint style="danger" %}
Remember that in CG2027, we've seen that to **size** the PMOS and NMOS, we are **only** changing its **width** but its length must remain the same.
{% endhint %}

So, when we see INV2x, INV4x, etc., it means the **size/drive strength** is being multiplied by scaling the width $$W$$ of _both_ transistors by that factor (2,4, etc).

* INV1x: NMOS = W$$ $1W$ $$, PMOS = $$ $2W$ $$2W (Total gate capacitance $$\approx3C_{\text{unit}}$$$$ $\approx 3 C_{unit}$ $$)
* INV2x: NMOS = 2W $$ $2W$ $$, PMOS = $$ $4W$ $$4W (Total gate capacitance $$\approx6C_{\text{unit}}$$ $$ $\approx 6 C_{unit}$ $$)
* INV4x: NMOS = 4W$$ $4W$ $$, PMOS = $$ $8W$ $$8W (Total gate capacitance $$ $\approx 12 C_{unit}$ $$$$\approx12C_{\text{unit}}$$)

{% hint style="success" %}
Again, as we have seen [above](lec-2-cmos-inverter.md#defining-with-exact-values), if in Cadence EDA tools, we have the exact value for $$W$$ and $$L$$, we can get the gate capacitance by just plugging in those numbers.
{% endhint %}

A bigger inverter directly implies the following three things:

1. **Bigger Drive Strength**: A wider channel allows more current ($$ $I_{ds}$ $$$$ $I_{ds}$ $$$$I_{\text{DS}}$$) to flow.
2. **Lower Resistance**: As the bigger size implies lower resistance of the MOSFET.
3. **Bigger Capacitance**: As the biiger size implies bigger capacitance of the MOSFET.

</details>

#### Inverter delay with the load

As we have seen in the [#inverter-with-a-load](lec-2-cmos-inverter.md#inverter-with-a-load "mention"), we propagation delay of an inverter should also take its **intrinsic capacitance** into account. Given that, we can also add the gate capacitance $$C_{\text{gin}}$$ for the sake of analysis later.

<figure><img src="../.gitbook/assets/inverter-with-load-simplified.png" alt="" width="541"><figcaption></figcaption></figure>

The formula for the propagation delay $$t_p$$ can be refactored to as follows by dividing both the nominator and the denominator with $$C_{\text{gin}}$$

$$
t_p = 0.69 R C_{\text{int}} \left(1 + \frac{C_L}{C_{\text{int}}} \right)
= 0.69 R C_{\text{int}} \left( 1 + \frac{C_L / C_{\text{gin}}}{C_{\text{int}} / C_{\text{gin}}} \right)
= 0.69 R C_{\text{int}} \left( 1 + \frac{f}{\gamma} \right)
$$

where we define two new terms:

1. **Effective fanout** ($$f$$): $$C_L\div C_{\text{gin}}$$.
2. **Self loading factor** ($$\gamma$$): $$C_{\text{int}}\div C_{\text{gin}}$$

{% hint style="danger" %}
$$\gamma$$ is **independent** of the **gate size** since both $$C_{\text{int}}$$ and $$C_{\text{gin}}$$ increase with [gate size](#user-content-fn-2)[^2] and thus it is only a function of **technology**.
{% endhint %}

Let $$t_{p0}=0.69RC_{\text{int}}$$ be the **intrinsic delay**, the inverter delay can be expressed by

$$
t_p=t_{p0}\left(1+\frac{f}{\gamma}\right)
$$

Thus, let's take a relook at our inverter chain.

<figure><img src="../.gitbook/assets/inverter-chain-relook.png" alt=""><figcaption></figcaption></figure>

We can thus get the following two formulas:

$$
t_{p,j} = t_{p0}\left(1 + \frac{f_j}{\gamma}\right) = t_{p0}\left(1 + \frac{C_{\text{gin},j+1}}{\gamma C_{\text{gin},j}}\right) \qquad f_j = \frac{C_{\text{gin},j+1}}{C_{\text{gin},j}}
$$

$$
t_{p,\text{chain}} = \sum_{j=1}^{N} t_{p,j} = t_{p0} \sum_{j=1}^{N}\left(1 + \frac{C_{\text{gin},j+1}}{\gamma C_{\text{gin},j}}\right), \qquad C_{\text{gin},N+1} = C_L
$$

[^1]: Can be thought of as the slope.

[^2]: Gate size is nothing but the sum of the width of NMOS and the width of PMOS.
