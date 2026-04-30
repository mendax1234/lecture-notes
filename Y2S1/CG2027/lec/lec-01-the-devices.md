# Lec 01 - The Devices

The building blocks in digital circuit design are the silicon semiconductor devices, more specifically

1. the diodes
2. the MOS
3. bipolar transistors

In this lecture, or more specifically, in this course, we will focus more on the MOS, but will quickly cover the diodes and bipolar transistors.

## The Diode

Although diodes rarely occur directly in the schematic diagrams of present-day digital gates, they are still omnipresent. For instance, each MOS transistor implicitly contains a number of reverse-biased diodes. Diodes are used to protect the input devices of an IC against static charges. Also, a number of bipolar gates use diodes as a means to adjust voltage levels.

### Introduction

The diode to be discussed here is a **semiconductor&#x20;**_**pn**_**-junction**. The _pn_-junction diode is the simplest circuit element of the semiconductor devices. Figure 1.1a shows a cross-section of a typical _pn-_&#x6A;unction.

<figure><img src="../.gitbook/assets/pn-junction-diode.png" alt="" width="375"><figcaption><p>Figure 1.1 Abrupt pn-junction diode and its schematic symbol</p></figcaption></figure>

* The _p_-type material is created by doping the silicon with _acceptor_ impurities (such as boron), which results in the presence of [**holes**](#user-content-fn-1)[^1] as the dominant or majority carriers.
* Similarly, the doping of silicon with _donor_ impurities (such as phosphorus or arsenic) creates an _n_-type material, where **electrons** are the majority carriers.

Aluminum contracts provide access to the _p-_ and _n-_&#x74;erminals of the device. The circuit symbol of the diode, as used in schematic diagrams, is introduced in Figure 1.1c.

<figure><img src="../.gitbook/assets/diode-doping.png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="danger" %}
In a semiconductor, there are two types of charge carriers: **electrons** (with&#x20;charge of -1.602×10<sup>-19</sup> C) and **holes** (with charge of +1.602×10<sup>-19</sup> C)
{% endhint %}

### Semiconductors

Unlike metal and insulator, a unique property of semiconductor is that&#x20;impurities can be added (in a controlled manner) into it. This is called _doping_ and its purpose is:

* to make the material _n-_&#x74;ype or _p-_&#x74;ype, and
* to change the material's conductivity (or resistivity), usually is to **increase** the conductivity.

In other words, we are changing the conductivity by specifically making the material _n-type_ or _p-type_. For more about the chemistry side of doping, please go [here](lec-01-the-devices.md#holes-and-electrons).

{% hint style="success" %}
**Increasing** a material's **conductivity** is equivalent to **decreasing** its **resistivity**.
{% endhint %}

### Origin of Current

In electronics, current is just the movement of charged particles (electrons or holes). This movement happens in two main ways: **Drift** and **Diffusion**.

<figure><img src="../.gitbook/assets/drift-diffuse.png" alt="" width="563"><figcaption></figcaption></figure>

{% stepper %}
{% step %}
#### Drift Current

As explained clearly in the image. There is an external electric field to drive the movement of the electrons and holes to the equilibrium.
{% endstep %}

{% step %}
#### Diffusion Current

Diffusion happens whenever something spreads out from where it’s concentrated to where it’s not, e.g., from high concentration to low concentration. In other words, diffusion is driven by the **difference in concentrations**, or the **concentration gradient**.

**Analogy**: recall that when we drop an ink into the water, it will start diffusing until it reaches a static state. Similarly here, under the **diffusion**, there is no external force, the holes and electrons will spontaneously move to the correct region to reach the equilibrium.
{% endstep %}
{% endstepper %}

{% hint style="success" %}
The current in all electronic devices originates from either of these two mechanisms.
{% endhint %}

#### Origin of Current in different devices

Below is the table summarizing the carrier movement in different devices

| Devices                     | Movement Mechanism in on-state | Type of Carriers                                                                |
| --------------------------- | ------------------------------ | ------------------------------------------------------------------------------- |
| Resistor                    | Drift                          | <ul><li>Electrons (Metal)</li><li>Electrons and holes (Semiconductor)</li></ul> |
| Diode                       | Diffusion                      | <ul><li>Electrons and holes</li></ul>                                           |
| Bipolar Junction Transistor | Diffusion                      | <ul><li>Electrons and holes</li></ul>                                           |
| MOSFET                      | Drift                          | <ul><li>Electrons (NMOS)</li><li>Holes (PMOS)</li></ul>                         |

### Operation

Diode (semiconductor pn-junction) is the simplest (2-terminal) and most&#x20;fundamental **nonlinear** circuit element.

* It allows a current flow through it easily **in one direction** (known as the [forward  &#x20;direction](lec-01-the-devices.md#forward-bias), V > 0), but **not in the opposite direction** (known as the [reverse direction](lec-01-the-devices.md#reverse-bias), V < 0), **except for the reverse** [**breakdown region**](lec-01-the-devices.md#breakdown-region)**.** This is unlike a resistor, which is a linear element that has a linear current-voltage relation.
* Diode can be used as a **switch** and in a **rectifier** circuit to convert AC into DC. (CG1111A!)

#### Forward Bias

<figure><img src="../.gitbook/assets/diode-forward-bias.png" alt=""><figcaption></figcaption></figure>

* Under **forward-bias** (V > 0), an external voltage is applied such that the _p-_&#x74;ype terminal is at a **higher** (positive) voltage with respect to the _n-_&#x74;ype terminal.
* The forward current flows through the diode from the _p-_&#x74;ype side to the _n-_&#x74;ype side.
* The forward current remains small (around 0 practically) until the **cut-in voltage** is reached. It then increases quickly with a small increase in the voltage V thereafter.
* With a substantial forward current, the **voltage drop** across the diode lies in a narrow range. In other words, the voltage drop almost remains constant.

#### Reverse Bias

<figure><img src="../.gitbook/assets/diode-reverse-bias.png" alt=""><figcaption></figcaption></figure>

* Under **reverse-bias**, the voltage at _p-_&#x74;ype is lower than the voltage at _n-_&#x74;ype.
* The reverse current (around 0) flows through the diode from the _n-_&#x74;ype side to the _p-_&#x74;ype side.
* For reverse bias voltage magnitude, |V| = V<sub>R</sub> < V<sub>Z</sub> (V<sub>Z</sub> is the **breakdown voltage**), the reverse current is very small and can be treated practically as **zero**, meaning the diode is equivalent to an **open circuit**.

#### Breakdown Region

<figure><img src="../.gitbook/assets/diode-breakdown-region.png" alt=""><figcaption></figcaption></figure>

* Current, while operating in the breakdown region, can be limited by connecting a resistor, $$R$$, of suitable value in series with the $$pn$$ junction diode. ($$I=\frac{V_{DD}-V_Z}{R}$$)
* Operation in the breakdown region does not destroy the diode, provided the current through it is kept below a certain level (because of the resistor in this circuit), such that the power dissipation ($$V×I$$) is below what the diode can handle.

### More under the hood

As we have mentioned above, the _pn-_&#x6A;unction or the diode is the **most fundamental** circuit element. We can also find _pn-_&#x6A;unction in the MOSFET transistor. So, it will be good to know what's happening underneath the hood.

#### Holes and Electrons

The main carrier in _n-_&#x74;ype material is **electron** while in _p-_&#x74;ype material is **hole**. But we should know that in whatever material, **only** the **electrons** can move freely, and the movement of electrons will cause the "effect" that **holes** are moving too, but actually the movement of holes is caused by the movement of **electrons**.

Both _n_-type and _p-_&#x74;ype material are neutral, by which I mean inside each material, the number of electrons and the number of **protons** are the same. The underneath principle is that:

{% columns fullWidth="false" %}
{% column %}
<p align="center"><strong>p-type — holes</strong></p>

<figure><img src="../.gitbook/assets/p-type-holes.png" alt=""><figcaption></figcaption></figure>

In _p-_&#x74;ype material, a new intermediate **energy-level** is created (<mark style="color:yellow;">yellow plate</mark> in the image above) slighly above the energy level which is below the gap so that the electrons (<mark style="color:blue;">blue spheres</mark> in the image above) will move up, leaving behind the [**holes**](#user-content-fn-2)[^2] in the lower energy level, making the electrons at the lower energy level free to move around. This can be seen as the **holes** moving around.

{% hint style="success" %}
We move some of the **original electrons** above the yellow plate. And the number of protons and electrons still remain the same.
{% endhint %}
{% endcolumn %}

{% column %}
<p align="center"><strong>n-type — electrons</strong></p>

<figure><img src="../.gitbook/assets/n-type-electrons.png" alt=""><figcaption></figcaption></figure>

In _n-_&#x74;ype material, this new intermediate energy level (<mark style="color:yellow;">yellow plate</mark> in the image above) is created at the energy level above the gap. And at this level, the electrons (<mark style="color:blue;">blue spheres</mark> in the image above) can flow freely.

{% hint style="success" %}
Here, both **new protons and electrons** are created together so that number of protons and electrons are still the same.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

{% hint style="warning" %}
Here, the height only shows the **energy level** of the **electrons**, not protons. The higher the level is, the higher energy the electrons at that level has.
{% endhint %}

The electrons moving at the **energy level** which is above the gap **will and only will** recombine with the holes at the **energy level** which is below the gap.

<details>

<summary>What does the gap here mean?</summary>

Just a quick recap about what the gap means. The gap is called the **band gap**. Basically, the bigger the gap is, the less conductive the material is. In the semiconductor material, the gap is relatively small. Even the gap is small, if we don't **dope** the semiconductor material, in other words, creating the above yellow plate, it's hard for the electrons to continuously moving freely in the semiconductor material.

* For _n-_&#x74;ype material, the yellow plate is called **donor energy level**.
* For _p-_&#x74;ype material, the yellow plate is called **acceptor energy level**.

By themselves, a piece of N-type or P-type silicon is just a resistor (a material that conducts electricity). The magic that makes all modern electronics work happens when we join them together to form a _pn-_&#x6A;unction. This is what creates the special properties of a diode, a transistor, and every computer chip.

</details>

The process to create this kind of <mark style="color:yellow;">yellow plate</mark> is called **doping**. But given that, it is still a bit blur to some of you maybe. So, let's use the high school knowledge to further explain what is doping.

{% stepper %}
{% step %}
#### Conductor

We know that some materials are good conductors, for example, **copper**. But why are they good conductors? It is because by looking at copper's atomic structure, we see that it has a **single electron** in its **outermost shell**.

<figure><img src="../.gitbook/assets/cupper-atom-level.png" alt="" width="563"><figcaption></figcaption></figure>

Suppose our wire is made up of copper. When a power source is applied, this free electron in the copper, under the influence of the **external electric field**, will move in a specific direction, thus producing an electric current.

<figure><img src="../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure>


{% endstep %}

{% step %}
#### Semiconductor

There is a type of material whose **conductivity** lies between the conductor and the insulator. One example is the **silicon** and its atomic structure is shown as follows,

<figure><img src="../.gitbook/assets/silicon-atom-level.png" alt="" width="563"><figcaption></figcaption></figure>

Silicon's outermost shell can actually hold 8 electrons, but one silicon atom only has 4 at its outermost shell. If we place multiple silicon atoms together, we will find that one silicon atom can share electron pairs with its four surrounding silicon atoms, forming **covalent bonds**. This is a very **stable** structure, which binds the electrons very strongly. Therefore, silicon's **conductivity** is very **weak** in this state.

<figure><img src="../.gitbook/assets/many-silicon.png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### N-type Doping

But if we dope silicon with the element **phosphorus** (which is element #15), phosphorus has 5 electrons in its outermost shell, four of which can form **covalent bonds** with four silicon electrons. The remaining one acts as **a free electron**, which increases the conductivity of the new material. This type of doping is called _**n-**_**type doping**. Therefore, in _n-_&#x74;ype material, the charge carriers are **electrons**.

<figure><img src="../.gitbook/assets/n-type-doping.png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
#### P-type doping

Similarly, if we dope silicon with the element **Boron** (which has 3 valence electrons), Boron's 3 outermost electrons can form **covalent bonds** with silicon, but this leaves an incomplete bond, which forms a **hole**. However, other electrons can move into this hole. Because another electron moved in, it is equivalent to the hole moving to a different location. At this point, the conductivity of this boron-doped silicon will also increase, and this type of doping is called _p-_&#x74;ype doping. Therefore, in _p-_&#x74;ype material, the charge carriers are **holes**.

<figure><img src="../.gitbook/assets/p-type-doping.png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

Now we have seen the basic structure of _n-_&#x74;ype and _p-_&#x74;ype material. Let's see some interesting things when we put them together, which is to form the _pn-_&#x6A;unction.

#### Depletion region

In the connection between the _n-_&#x74;ype material and _p-_&#x74;ype material, the excessive and **diffused** electrons from the _n-_&#x74;ype flows to the _p-_&#x73;ide and recombine with the excessive **holes** in the _p-_&#x74;ype side. As a result, some charge carriers (free electrons for the _n-_&#x74;ype material and holes for the _p-_&#x74;ype material) are depleted in the region around the junction interface, so this region is called the **depletion region**.

{% hint style="success" %}
Recall that in diode/_pn-_&#x6A;unction, current is formed because of **diffusion**.
{% endhint %}

#### Diffusion in Diode

The depletion region is **charged** because the **diffusion** described above will cause the _p-_&#x73;ide to have **more electrons** thus showing **negative charge** and the _n-_&#x74;ype side to have **less electrons** thus showing **positive charge**. This creates an **electric field** pointing from the positive charge area to the negative charge area, thus providing a force **opposing** the charge **diffusion**. When the electric field is sufficiently strong to cease further diffusion of holes and electrons, the depletion region reaches the equilibrium.

<figure><img src="../.gitbook/assets/diode-workinig-principle.png" alt="" width="563"><figcaption></figcaption></figure>

#### Forward and Reverse Bias combined with Diffusion

If we apply **lower voltage** at the _n-_&#x73;ide, the external electric field will cancel out the inner electric field, thus the depletion region will be **decreased** and the external field will help the movement of the electrons, making it easier for the electrons to move from _n-_&#x73;ide to _p_-side. This is called **forward bias**,

<figure><img src="../.gitbook/assets/forward-bias-under-hood.png" alt=""><figcaption></figcaption></figure>

Similarly, if we apply higher voltage at _n-_&#x73;ide, the external electric field will reinforce the inner electric field, thus **increasing** the depletion region, making it harder for the electrons to move from _n-_&#x73;ide to _p_-side. This is called **reverse bias**.

<figure><img src="../.gitbook/assets/reverse-bias-under-hood.png" alt=""><figcaption></figcaption></figure>

## BJT

### Introduction

<figure><img src="../.gitbook/assets/bjt-introduction.png" alt=""><figcaption></figcaption></figure>

* Bipolar junction transistor (BJT) is a **3-terminal** device made using a single crystal semiconductor (typically silicon), just like the pn-junction diode.
* BJT is made with **3 doped semiconductor regions**, namely **emitter**, **base** and **collector**, corresponding to the 3 terminals.
* The "active" region of the BJT is the region under (and including) the **emitter**.
* BJT is not a symmetrical device, in particular, impurities added to the **emitter** is at a much higher concentration than that added to **collector**.

### Modes of Operation

The Bipolar Junction Transistor (BJT) has two **pn-junctions**: the emitter-base junction and the collector-base junction. Each of these junctions can be either **forward biased** or **reverse biased**. However, we will not focus on the BJT, as it is not used in CMOS logic circuits.

## MOSFETs

The metal-oxide-semiconductor field-effect transistor (MOSFET, or MOS, for short) is certainly the workhorse of contemporary digital design. Its major asset, from a digital perspective, is that is performs very well as a **switch** with an **infinite off-resistance** (for |V<sub>GS</sub>| < |V<sub>TH</sub>|) and a **finite on-resistance** (for |V<sub>GS</sub>| < |V<sub>TH</sub>|).

<figure><img src="../.gitbook/assets/mos-as-switch.png" alt="" width="563"><figcaption></figcaption></figure>

Before moving on, let's make some conventions

1. For MOSFETs, S denotes **Source**, D denotes **drain**, G denotes **gate**, and B denotes the **body** or subtrate terminals.
2. For simplicity, we assume body (substrate) is shorted to source terminal.
3. **Source** and **drain** are physically **symmetrical**.
4. Majority charge **carriers** move **from source to drain**.
5. V<sub>GS</sub>: **Gate to source** voltage. Or the difference between gate voltage and source voltage. (Treat it as a vector in math, so V<sub>GS</sub>=-V<sub>SG</sub>)
6. V<sub>DS</sub>: **Drain to source** voltage.
7. V<sub>DD</sub>: Supply voltage.
8. V<sub>T</sub>: Threshold voltage.

### Introduction

There are two types of MOSFET: n-channel and p-channel MOSFETs, or to put it simply, just NMOS and PMOS.

<figure><img src="../.gitbook/assets/nmos-basic-structure.png" alt="" width="563"><figcaption></figcaption></figure>

* An **n-channel** MOSFET or NMOS is made using a _**p-**_**type** single-crystal silicon **substrate**.
* Heavily doped _**n**_<sup>_**+**_</sup>_**-**_**type** regions, created in the substrate, form the **source** and **drain** regions.
* The metal or polysilicon electrode on top of the thin oxide (dielectric) layer, between the **source** and **drain** regions, is called the **gate**.
* Note that MOSFET has a fourth terminal, which is the **substrate** or **body**.
* **Source terminal** is the **source of the carriers** that will flow through the channel to the **drain terminal**.
  * **Analogy**: In NMOS, we can think the **drain terminal** as something that sucks the **electrons** from the source (ground) to the drain. We will use this analogy in the later parts also.

### Physical Structure

<figure><img src="../.gitbook/assets/mosfet-physical-structure.png" alt=""><figcaption></figcaption></figure>

* **Substrate (body)**: Lightly doped
* **Source, Drain**: Heavily doped charge wells; symmetric
* **Gate oxide (dielectric)**: Insulator between gate and channel
* **Gate**: Controls the charge flow (creates the field effect)
* **Gate length**: the distance between the **source** and **drain** regions under the gate

{% columns %}
{% column width="50%" %}
<figure><img src="../.gitbook/assets/nmos-channel.png" alt=""><figcaption></figcaption></figure>

* _n-_&#x74;ype source/drain
* _p-_&#x74;ype body (substrate)
* **Electrons** flow from source to drain
* Current flows from **drain to source**
* We can memorize it using "npn".

<figure><img src="../.gitbook/assets/nmos-schematic.png" alt=""><figcaption></figcaption></figure>

* Source and body are usually connected to ground (0V)
* V<sub>T</sub> is **positive**.
* When V<sub>GS</sub> $$\geq$$ V<sub>T</sub>, device is on.
* Linear region of operation: 0 < V<sub>DS</sub> < V<sub>GS</sub> - V<sub>T</sub>
* V<sub>Dsat</sub> = V<sub>GS</sub> - V<sub>T</sub>
{% endcolumn %}

{% column width="50%" %}
<figure><img src="../.gitbook/assets/pmos-channel.png" alt=""><figcaption></figcaption></figure>

* _p-_&#x74;ype source/drain
* _n-_&#x74;ype body (substrate)
* **Holes** flow from source to drain
* Current flows from **source to drain**
* Similarly, we can memorize it using "pnp"

<figure><img src="../.gitbook/assets/pmos-schematic.png" alt=""><figcaption></figcaption></figure>

* Source and body are usually connected to V<sub>DD</sub>
* Drain is **not** usually connected to ground (0V).
* V<sub>T</sub> is **negative**.
* When V<sub>GS</sub> $$\leq$$ V<sub>T</sub>, device is on.
* Linear region of operation: 0 > V<sub>DS</sub> > V<sub>GS</sub> - V<sub>T</sub>
* V<sub>Dsat</sub> = V<sub>GS</sub> - V<sub>T</sub>
{% endcolumn %}
{% endcolumns %}

{% hint style="success" %}
#### Notes

1. The threshold voltage, V<sub>T</sub>, is the gate-source voltage at which channel is formed.
   1. The **channel** in a MOSFET is the path that allows current to flow between the source and the drain — under the gate oxide layer. It forms inside the semiconductor substrate (usually silicon) when we apply a voltage to the gate.
2. For the sake of simplicity, we made the following rules when analyzing the MOSFET.
   1. NMOS and PMOS are ON when the |V<sub>GS</sub>| > |V<sub>TH</sub>| and OFF when |V<sub>GS</sub>| <i class="fa-less-than-equal">:less-than-equal:</i> |V<sub>TH</sub>|
   2. If we increase |V<sub>GS</sub>|, the channel will become wider, thus the output current I<sub>D</sub> will increase.
   3. To determine the **linear region** or the **saturation region**, we must strictly following the following table. For the V<sub>GS</sub> and V<sub>TH</sub> relationship we can use the **absolute thinking** mentioned above. For the V<sub>DS</sub>, V<sub>GS</sub> and V<sub>TH</sub>, we can use the [I-V characteristcs](lec-01-the-devices.md#output-characteristic) diagram introduced later!
{% endhint %}

<figure><img src="../.gitbook/assets/cmos-region reference table.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
We can understand the condition for the NMOS and then flip all the signs when dealing with PMOS.
{% endhint %}

For the sake of simplicity, in this course, we will use NMOS as an example. And thus the following sections will be based on NMOS. The PMOS equivalent will be left as an exercise to the reader.

### Modes of Operations

We may notice that between the **source**, **drain** and the **subtrate**, there are always _**pn**_**-junctions** (<mark style="color:purple;">purple area</mark> in the image below in this section). This is very important and it affects how our electrons can move.

#### Cut-off

We start when V<sub>GS</sub> = 0 -> no **channel** is formed for V<sub>DS</sub> $$\geq$$ 0 -> No current flow.

<figure><img src="../.gitbook/assets/nmos-cut-off.png" alt="" width="563"><figcaption></figcaption></figure>

#### In between cut-off and linear

We then increase V<sub>GS</sub> unilt V<sub>GS</sub> = V<sub>TH</sub>.

<figure><img src="../.gitbook/assets/nmos-between-cut-off-and-linear.png" alt="" width="563"><figcaption></figcaption></figure>

So what happens physically is that: At this point of time, **n-channel** is formed between source to drain. This is because as we increase V<sub>GS</sub>, and with the **existence of the insulator (oxide),** the high voltage applied at V<sub>GS</sub> pushes away more holes and **attracts more electrons** to pile up near the surface of the subtrate under the gate. So the surface of **p-subtrate** effectively becomes **n-type** and is said to be **inverted** leading to formation of **n-channel**. The gate voltage at which this inversion happens is called **Threshold voltage, V**<sub>**TH**</sub>**.** As you may notice, below the newly formed **n-channel**, the new _pn-_&#x6A;unction is formed, but in this case, it is in the **reverse-biased** state.

#### Linear

After that,

1. we continue increasing V<sub>GS</sub> until it is bigger than V<sub>TH</sub>. And then we stop increasing V<sub>GS</sub> and fix it.
2. Then we slowly increase V<sub>DS</sub> but it shouldn't exceed V<sub>GS</sub>-V<sub>TH</sub>.

So now, as V<sub>DS</sub> > 0, and there is an **n-channel** in between source and drain, as we have written above, the current will flow **from drain to source**.

<figure><img src="../.gitbook/assets/linear-region.png" alt="" width="563"><figcaption></figcaption></figure>

At point x along the channel, the voltage is V(x), and the gate-to-channel voltage at that point equals V<sub>GS</sub> - V(x). We assume that under linear region, this voltage **exceeds** the **threshold voltage** all along the channel. So again, what happens physically here is that:&#x20;

* Within the NMOS, a conductive **n-type channel** is formed between the **source** and **drain**.
* When V<sub>DS</sub>​ (drain-to-source voltage) is small, the entire channel is uniformly formed — electrons can move easily.
* As V<sub>DS</sub>​ increases slightly, the **electric field** pushes more electrons, and **current I**<sub>**D**</sub>**​** increases **proportionally** (Ohm's law). This is valid only for **small values of V**<sub>**DS**</sub>.
* This is called **linear region** or **resistive region** of a MOSFET.

{% hint style="success" %}
"Region" means a range of V<sub>GS</sub> and V<sub>DS</sub> values where the MOSFET shows a specific physical behavior and current-voltage or I-V relationship.
{% endhint %}

#### In between linear and saturation

Now, as we have already fixed the V<sub>GS</sub> and start increasing the V<sub>DS</sub>, we increase V<sub>DS</sub> until V<sub>DS</sub> = V<sub>GS</sub>-V<sub>TH</sub>. As the value of drain-source voltage V<sub>DS</sub> is further increased, the assumption that the channel voltage is larger than the threshold all along the channel ceases to hold. This happens when V<sub>GS</sub> - V(x) < V<sub>TH</sub>. At that point, the induced charge is zero, and the conducting channel disappears or is **pinched-off**.

<figure><img src="../.gitbook/assets/nmos-in-between-linear-saturation.png" alt="" width="563"><figcaption></figcaption></figure>

So, what happens physically now? The effective voltage between gate and source V<sub>GD</sub> = V<sub>G</sub> - V<sub>D</sub> = V<sub>GS</sub> - V<sub>DS</sub> (As V<sub>S</sub> = 0) = V<sub>GS</sub> - (V<sub>GS</sub> - V<sub>TH</sub>) = V<sub>TH</sub>. Then the channel at drain end begins to **pinch off**.

{% hint style="success" %}
Using the sucking analogy on the drain side, we can think of it as when we increase V<sub>DS</sub>, the drain will suck the electrons **more quickly**, thus the channel near to the drain will become **thinner**. As the drain cosumes the electrons that the source provided, this is also why the source pin is called source and the drain is called drain.
{% endhint %}

#### Saturation

As we continue increasing V<sub>DS</sub> until V<sub>DS</sub> > V<sub>GS</sub> - V<sub>TH</sub>. Let's denote three variables here:

1. $$L$$ is the channel length
2. $$\Delta L$$ is pinched-off channel length
3. $$L_{\text{eff}}$$ is the effective channel length = $$L-\Delta L$$

<figure><img src="../.gitbook/assets/nmos-saturation.png" alt="" width="563"><figcaption></figcaption></figure>

So, what happens physically? Although the channel at the drain end pinches off, the electrons still flow to drain under the influence of **high electric field in the pinch-off region** from drain to source and hence the **current is constant**.

{% hint style="warning" %}
Here, in the **pinched-off** region near the drain, there is still **n-channel**, it's just that it is very **thin**, it doesn't mean that there is no **n-channel**.
{% endhint %}

<details>

<summary>What are the short channel effects?</summary>

Basically, when transistors get **very tiny**, the gate **loses some control** over the channel, and new unwanted effects appear — those are called **short-channel effects (SCEs).**

</details>

***

In summary, we have the following table summarizing the above 5 stages of NMOS.

| Condition / VDS Range                                    | Region                                     | Channel Description                                                                 | Current Behavior / Equation                                           |
| -------------------------------------------------------- | ------------------------------------------ | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
|  $$V_{GS} < V_{TH}$$                                     | **Cutoff (OFF)**                           | No inversion channel formed; MOSFET is OFF                                          |  $$I_D \approx 0$$                                                    |
| $$V_{GS} \approx V_{TH}$$                                | **Boundary between Cutoff and Linear**     | Channel **just starts to form** (weak inversion); small leakage current begins      | $$I_D$$ rises exponentially with $$V_{GS}$$ (subthreshold conduction) |
| $$V_{GS} > V_{TH}$$ and $$0 < V_{DS} < V_{GS} - V_{TH}$$ | **Linear (Ohmic)**                         | Channel fully formed and uniform; acts like voltage-controlled resistor             | $$I_D =f(V_{DS}, V_{GS})$$                                            |
| $$V_{DS} = V_{GS} - V_{TH}$$                             | **Boundary between Linear and Saturation** | Channel begins to pinch off at the drain end                                        | Marks start of current saturation                                     |
| $$V_{GS} > V_{TH}$$ and $$V_{DS} > V_{GS} - V_{TH}$$     | **Saturation (Active)**                    | Channel pinched off near drain; current nearly constant (independent of $$V_{DS}$$) | $$I_D =f(V_{GS})$$                                                    |

<details>

<summary>In NMOS, why the current only flows from drain to source?</summary>

We can think of the three possible routes that the current might flow:

1. **Gate to other places**: This is **not possible** as there is an insulator material (oxide) between gate and source, drain and subtrate)
2. **Source to body**: This is **not possible** as the source and body are both connected to ground (0V). And because of the existence of the **depletion region**. No voltage difference plus the depeltion region, the current cannot flow from source to body.
3. **Drain to body**: As body is 0V and drain is connected to **positive voltage**, because of the **reverse-bias** property of the _pn-_&#x6A;unction, the current cannot flow from drain to body. And as you increase V<sub>DS</sub>, the depletion region can only become bigger and bigger, making it even more impossible for the current to flow from the drain to the body.

</details>

### I-V characteristics

#### Output characteristic

{% stepper %}
{% step %}
#### Linear Region

In the linear region, we have two situations

* Increase in V<sub>DS</sub> leads to increase in current like **voltage-controlled resistor**.
* Increase in V<sub>GS</sub> leads to a greater number of electrons leading to higher current.

That's why $$I_D =f(V_{DS}, V_{GS})$$, and the function $$f$$ is

$$
i_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GS} - V_T)V_{DS} - \frac{1}{2}V_{DS}^2 \right] \\
$$

where

$$
\begin{align*}
\mu_n      & \quad \text{is the electron mobility in the channel} \\
C_{ox}    & \quad \text{is the Gate oxide capacitance per unit area} \\
W         & \quad \text{is the Channel Width} \\
L         & \quad \text{is the Channel length}
\end{align*}
$$

Based on this equation, we see that $$I_D$$ doesn't increase **perfectly** linearly with $$V_{DS}$$. Only when $$V_{DS}$$ is very small -> $$\frac{1}{2}V_{DS}^2$$ is very negligible -> we can say the increasing is **linear**.

{% hint style="info" %}
The linear region is where the digital integrated design comes in.
{% endhint %}
{% endstep %}

{% step %}
#### Saturation Region

In the saturation region, only the increase in V<sub>GS</sub> will affect the current. We denote V<sub>Dsat</sub> = (V<sub>GS</sub> - V<sub>T</sub>).

<figure><img src="../.gitbook/assets/iv-characteristics-saturation-region.png" alt="" width="375"><figcaption></figcaption></figure>

That's why we have $$I_D =f(V_{GS})$$, and the function $$f$$ is

$$
i_D = i_{Dsat} = \frac{1}{2}\mu_n C_{ox} \frac{W}{L} (V_{GS} - V_T)^2
$$

where,

$$
\begin{align*}
\mu_n      & \quad \text{is the electron mobility in the channel} \\
C_{ox}    & \quad \text{is the Gate oxide capacitance per unit area} \\
W         & \quad \text{is the Channel Width} \\
L         & \quad \text{is the Channel length}
\end{align*}
$$

{% hint style="info" %}
The saturation region is where the analog integrated design comes in.
{% endhint %}

<details>

<summary>More on Capacitance</summary>

1. $$C_{ox}=\frac{\epsilon_0\epsilon_r}{t_{ox}}$$, where $$t_{ox}$$ is the thickness of the gate oxide. And sometimes $$\epsilon_0\epsilon_r$$ will be written as $$\epsilon_{ox}$$. They are the same.
2. The gate capacitance: $$C_{\text{gate}}=C_{ox}\cdot W\cdot L=\frac{\epsilon_{ox}}{t_{ox}}\cdot W\cdot L$$.

</details>
{% endstep %}
{% endstepper %}

In summary, below is the NMOS and PMOS I-V characteristics (i<sub>D</sub> and V<sub>DS</sub>), where the red cicrled part is the **linear region** and the green circled part is the **saturation region**. This is called **output characteristics**, where I<sub>D</sub> versus V<sub>DS</sub> with V<sub>GS</sub> as a parameter.

<figure><img src="../.gitbook/assets/nmos-pmos-iv-characteristics.png" alt=""><figcaption></figcaption></figure>

To put this diagram together, we have the following

<figure><img src="../.gitbook/assets/iv-diagram-nmos-pmos.png" alt="" width="563"><figcaption></figcaption></figure>

From this we can see that, if we fix our V<sub>GS</sub>, and increase the magnitude of the V<sub>DS</sub>, the PMOS/NMOS will enter the saturation region. Respectively speaking,

1. For NMOS, if we increase V<sub>DS</sub>, we will enter the saturation region -> one of the conditions for saturation is V<sub>DS</sub> > V<sub>GS</sub> - V<sub>TN</sub>, which is the same as V<sub>GD</sub> < V<sub>TN</sub>.
2. For PMOS, if we decrease V<sub>DS</sub>, we will enter the saturation region -> one of the conditions for saturation is V<sub>DS</sub> < V<sub>GS</sub> - V<sub>TP</sub>, which is the same as V<sub>GD</sub> > V<sub>TP</sub>.

#### Transfer characteristic

If we draw the I-V characteristics between the V<sub>GS</sub> and i<sub>D</sub>, we will find that after V<sub>GS</sub> exceeds beyond V<sub>TH</sub>, i<sub>D</sub> will increase **quadratically** as V<sub>GS</sub> increases. This is called **transferred characteristics**, where i<sub>D</sub> versus V<sub>GS</sub> at a given fixed V<sub>DS</sub> value (V<sub>DS</sub> is chosen so that the MOSFET is in **saturation region**).

<figure><img src="../.gitbook/assets/transfer-characteristics-of-mosfets.png" alt=""><figcaption></figcaption></figure>

So, to understand its geometric meaning, we can see from the following graph,

<figure><img src="../.gitbook/assets/meaning-transfer-characteristic.png" alt=""><figcaption></figcaption></figure>

### New Transistor Architecture

As for now, we only have one gate to control the channel. Nowadays, the new transistor architecture is called 3D FET.

<figure><img src="../.gitbook/assets/3d-fet.png" alt=""><figcaption></figcaption></figure>

For example, the following is the diagram showing the **height**, **width** and **length** of FinFET and Gate-All-Around.

<figure><img src="../.gitbook/assets/finfet.png" alt="" width="420"><figcaption><p>FinFet length, width and height</p></figcaption></figure>

## Misc

### Timing Diagram

In CG2027, only the following 4 terms will be covered and included in the final test. Thus, knowing how to calculate each of them will be necessary for succeed in this course.

<figure><img src="../.gitbook/assets/timing-diagram.png" alt="" width="563"><figcaption></figcaption></figure>

1. t<sub>PHL</sub>: output high to low but related to input's effect on output; see the 50% point of V<sub>out</sub> and V<sub>in</sub>
2. t<sub>PLH</sub>: output low to high but related to input's effect on output; also look at the 50% point of V<sub>out</sub> and V<sub>in</sub>
3. t<sub>f</sub>: output high to low, look at output only; 10% and 90% point of V<sub>out</sub>
4. t<sub>r</sub>: output low to high, look at output only; 10% and 90% point of V<sub>out</sub>

### Unit Table

In the final, memorize the unit table is necessary for some questions!

| Factor | Name  | Symbol |
| ------ | ----- | ------ |
| 10⁹    | Giga  | G      |
| 10⁶    | Mega  | M      |
| 10³    | Kilo  | k      |
| 10²    | Hecto | h      |
| 10¹    | Deca  | da     |
| 10⁻¹   | Deci  | d      |
| 10⁻²   | Centi | c      |
| 10⁻³   | Milli | m      |
| 10⁻⁶   | Micro | µ      |
| 10⁻⁹   | Nano  | n      |
| 10⁻¹²  | Pico  | p      |
| 10⁻¹⁵  | Femto | f      |
| 10⁻¹⁸  | Atto  | a      |
| 10⁻²¹  | Zepto | z      |

## Homework

### Some awesome videos

Great thanks to my tutor **juezhao**, the following two videos from YouTube explains perfectly on everything under the hood, from the movement of electrons to the _pn-_&#x6A;unction and then to the MOSFETS.

{% embed url="https://www.youtube.com/watch?v=hrpPKCDLRN0" %}

{% embed url="https://www.youtube.com/watch?v=rkbjHNEKcRw" %}

Besides the above, the following video from bilibili is also awesome!

{% embed url="https://www.bilibili.com/video/BV1nL411x7jH/?share_source=copy_web&vd_source=38953bcabbabbab600e123d8740d5a8a" %}

[^1]: "holes" refers to electron holes, which are quasiparticles representing the absence of an electron in a material like a semiconductor. These holes act as **positive charge** carriers and are crucial for modern electronics, such as diodes and transistors.

[^2]: the element where there is a **proton** (<mark style="color:red;">red spheres</mark> shown in the image below) but without an electron. Thus, a hole carries **positive** charge.
