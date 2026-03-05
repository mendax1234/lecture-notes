# Lec 1 - MOSFET and CMOS Process

## Background

This part requires you to have a good command of [CG2027 Lec 1 Device](https://app.gitbook.com/s/6nPr3SObC3azazbFhfgF/lec/lec-01-the-devices), while you will also gain a deeper understanding of certain parts in CG2027.

### Semiconductors

**Semiconductors** are made mainly by **doping** the silicon (Group IV) with either dopants from Group V or dopants from Group III. This will give us two types of semiconductors:

1. **n-type**: Dopants are from Group V elements. The **free carriers** are **electrons**. Current flows in channel due to flow of free **electrons**.
2. **p-type**: Dopants are from Group III elements. The **free carriers** are **holes**. Current flows in channel due to flow of free **holes**.

{% hint style="warning" %}
The [**concentration** of **free carriers**](#user-content-fn-1)[^1] determine the resistivity of the semiconductor.
{% endhint %}

### The MOS Capacitor

#### The MOS Capapictor with P-substrate

There are 4 stages for the MOS capacitor with P-substrate and the **stage transformation** is done by increasing  $$V_{\text{gate}}$$.

<figure><img src="../.gitbook/assets/mos-capacitor-p-substrate.png" alt=""><figcaption></figcaption></figure>

{% stepper %}
{% step %}
#### Accumulation

When $$V_{\text{gate}}<0$$, we have a large amount of **electrons** accumulating at the upper side of the Oxide. This will attract the **holes** with positive charge to the lower side of the Oxide. As a result, the **concentration** of the holes, which are the **free carriers** in the p-type semiconductor, **increases** at the top of the p-substrate. This makes the top of the p-substrate **a lot more "**&#x70;-typed".
{% endstep %}

{% step %}
#### $$V_{\text{gate}}=0$$

In this stage, we increase $$V_{\text{gate}}$$ from negative to 0. Now, there is no electron at the gate. Thus, the **concentration** of **holes** in the p-substrate is **balanced**.
{% endstep %}

{% step %}
#### Depletion

When we increase $$V_{\text{gate}}$$  to $$0<V_{\text{gate}}<V_{\text{TN}}$$, there will be a **some** **positive charges** at the gate, causing the **holes** in the p-substrate to be pushed to its bottom. This will cause a special region called the **depletion region** to be formed at the top of the p-substrate.

{% hint style="success" %}
#### Depletion Region

We can just think of the **depletion region** as the region where there is **few/no free carriers**. In this case, as the free carriers — holes — of p-substrate is pushed away by the positive charges in the gate, there is few holes in the top region of the p-substrate.
{% endhint %}
{% endstep %}

{% step %}
#### Inversion

When we increase $$V_{\text{gate}}$$ to $$V_{\text{gate}}>V_{\text{TN}}$$, there will be **a large amount** of **positive charges** at the gate, causing **more** holes to be pushed away. **And more electrons will be attracted** to the top of the p-substrate. This will cause a special layer called **inversion layer** to be formed at the top of the p-substrate.

{% hint style="success" %}
#### Inversion Layer

The **inversion layer** can be think of the region/layer where there is a **high concentration** of **free minority carriers**. In this case, the free minority carriers in p-substrate are **electrons**.$$V_{\text{gate}}>V_{\text{TN}$$
{% endhint %}
{% endstep %}
{% endstepper %}

#### The MOS Capacitor with N-Substrate

<figure><img src="../.gitbook/assets/mos-capacitor-n-substrate.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Note that for p-type semiconductor, the threshold voltage $$V_{\text{TP}}$$ is **negative**.
{% endhint %}

The derivation is very similar to the p-substrate and thus it is left as an exericse for the reader.

> TODO: Update the Depletion Region in the PN Junction a.k.a diode.

### Classification of FETs

The classification of FETs can be shown as follows.

<figure><img src="../.gitbook/assets/fets-classification.png" alt=""><figcaption></figcaption></figure>

As you can see, our focus in EE4415 will be the MOSFETS under the **enhancement mode**.

{% hint style="success" %}
#### Enhancement vs. Depletion mode

* **Enhancement**: Electrical stimulus ($$V_{\text{gate}}$$) applied to create **inversion layer** in channel.
* **Depletion**: Electrical stimulus ($$V_{\text{gate}}$$) applied to **deplete** channel of free carriers.
{% endhint %}

[^1]: In other words, can simply think of it as the **number of free carriers**.
