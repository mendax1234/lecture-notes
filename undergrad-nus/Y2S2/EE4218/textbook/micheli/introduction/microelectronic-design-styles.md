# Microelectronic Design Styles

Different **design styles**, often called **methodologies**, have been used for **microelectronic circuits**. They are usually classified as (full) **custom** and **semicustom design styles**.

## (Full) Custom Design

In the (full) custom design style, the **functional** and **physical design** are **handcrafted**, requiring an **extensive effort** of a **design team** to optimize each **detailed feature** of the circuit. In this case, the design effort and cost are **high**, often compensated by the achievement of **high-quality circuits**. It is obvious that the **cost** has either to be **amortized** over a **large volume production** (as in the case of **processor design**) or **borne in full** (as in the case of **special applications**). **Custom design** was popular in the **early years of microelectronics**. Today, the **design complexity** has confined **custom design techniques** to **specific portions** of a **limited number of projects**, such as **execution units** and **floating point units** of some **processors**.

## Semicustom Design

**Semicustom design** is based on the concept of restricting the **circuit primitives** to a **limited number**, and therefore **reducing** the possibility of **fine-tuning** all parts of a circuit design. The restriction allows the **designer** to leverage **well-designed** and **well-characterized primitives** and to focus on their **interconnection**. The reduction of the possible number of **implementation choices** makes it easier to develop **CAD tools** for **design** and **optimization**, as well as reducing the **design time**.

The **loss in quality** is often **very small**, because:

* **fine-tuning** a **custom design** may be **extremely difficult** when the design is **large**, and
* **automated optimization techniques** for **semicustom styles** can explore a **much wider class of implementation choices** than a **designer team** can afford.

Today, the number of **semicustom designs** outnumbers **custom designs**. Recently, some **high-performance microprocessors** have been designed **fully** (and **partially**) using **semicustom styles**.

**Semicustom designs** can be partitioned into **two major classes**:

1. **cell-based design** and
2. **array-based design**.

These classes further subdivide into **subclasses**, as shown in **Figure 1.2**.

<figure><img src="../../.gitbook/assets/semi-custom-design (1).png" alt=""><figcaption><p>Figure 1.2 Semicustom design styles: a taxonomy</p></figcaption></figure>

### Cell-based Design

**Cell-based design** leverages the use of **library cells**, that can be designed once and stored, or the use of **cell generators** that synthesize **macro-cell layouts** from their **functional specifications**. In cell-based design, the **manufacturing process** is **not simplified** at all with respect to **custom design**. Instead, the **design process** is simplified, because of the use of **ready-made building blocks**.

#### Standard-Cell Design

**Cell-based design styles** include **standard-cell design** (Figure 1.3). In this case, the **fundamental cells** are stored in a **library**. Cells are designed once, but **updates** are required as the progress in **semiconductor technology** allows for **smaller geometries**. Since every cell needs to be **parametrized** in terms of **area** and **delay** over ranges of **temperatures** and **operating voltages**, the **library maintenance** is far from a **trivial task**.

<div align="center"><figure><img src="../../.gitbook/assets/standard-cell-design (1).png" alt="" width="375"><figcaption><p>Figure 1.3 One of AT&#x26;T's Application Specific Standard Product chips. The chip was designed and laid out using AT&#x26;T CAD tools, with a standard cell design style. (Courtesy of AT&#x26;T.)</p></figcaption></figure></div>

The user of a **standard-cell library** must first conform his or her design to the **available library primitives**, a step called **library binding** or **technology mapping**. Then, cells are **placed** and **wired**. All these tasks have been **automated**. An extension is the **hierarchical standard-cell style**, where **larger cells** can be derived by combining **smaller ones**.

A major advantage of **cell-based design** (**standard** and **macro-cells**) is the **compatibility with custom design**. Indeed, **custom components** can be added to a **semicustom layout**, and vice versa. The combination of **custom design** and **cell-based design** has been used often for **microprocessor design**, and is also referred to as **structured custom design** (Figure 1.4).

<figure><img src="../../.gitbook/assets/structured-custom-design (1).png" alt="" width="375"><figcaption><p>Figure 1.4 Microphotograph of the Alpha AXP chip by Digital Equipment Corporation, using several macro-cells designed with proprietary CAD tools. (Courtesy of Digital Equipment Corporation.)</p></figcaption></figure>

#### Macro-cell based Design

**Macro-cell–based design** consists of combining **building blocks** that can be synthesized by **computer programs**, called **cell** or **module generators**. These programs vary widely in **capabilities** and have **evolved tremendously** over the last **two decades**. The first generators targeted the automatic synthesis of **memory arrays** and **programmable logic arrays (PLAs)**. Recently, **highly sophisticated generators** have been able to **synthesize the layout** of various circuits with a **device density** and **performance** equal or **superior** to that achievable by **human designers**.

The user of **macro-cell generators** has just to provide the **functional description**. **Macro-cells** are then **placed** and **wired**. Even though these steps have been **automated**, they are inherently **more difficult** and may be **less efficient** as compared to **standard-cell placement and wiring**, due to the **irregularity in size** of the **macro-cells**.

### Array-Based Design

**Array-based design** exploits the use of a **matrix of uncommitted components**, often called **sites**. Such components are then **personalized** and **interconnected**. **Array-based circuits** can be further classified as **prediffused** and **prewired**, also called **mask programmable** and **field programmable gate arrays**, respectively (**MPGAs** and **FPGAs**).

#### Prediffused

In the prediffused case, **batches of wafers**, with **arrays of sites**, are **manufactured**. The **chip fabrication process** entails **programming the sites** by **contacting them to wires**, i.e., by **manufacturing the routing layers**. As a result, only the **metal** and **contact layers** are used to **program the chip**, hence the name "**mask programmable**." **Fewer manufacturing steps** correlate to **lower fabrication time** and **cost**. In addition, the **cost** of the **prediffused batches of wafers** can be **amortized** over several **chip designs**.

#### Prewired

**Prewired gate arrays** have been introduced recently and are often called "**field programmable gate arrays**" because these can be **programmed in the field**, i.e., **outside the semiconductor foundry**. They consist of **arrays of programmable modules**, each having the capability of implementing a **generic logic function**.

{% hint style="success" %}
More on FPGAs can be found in [Harris & Harris DDCA](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/digital-building-blocks/logic-arrays#field-programmable-gate-array).
{% endhint %}

## Comparison

We would now like to compare the **different design styles** in terms of **density**, **performance**, **flexibility**, **design** and **manufacturing times**, and **cost** (for both **low** and **high volumes**). **Manufacturing time** of **array-based circuits** is the time spent to **customize them**. The comparison is reported in the following **table**, which can serve as a **guideline** in choosing a **style** for a given **application** and **market**:

|                        | CustomCell-based | Prediffused | Prewired | Density    |
| ---------------------- | ---------------- | ----------- | -------- | ---------- |
| **Density**            | Very High        | High        | High     | Medium-Low |
| **Performance**        | Very High        | High        | High     | Medium-Low |
| **Flexibility**        | Very High        | High        | Medium   | Low        |
| **Design time**        | Very Long        | Short       | Short    | Very Short |
| **Manufacturing time** | Medium           | Medium      | Short    | Very Short |
| **Cost - low volume**  | Very High        | High        | High     | Low        |
| **Cost - high volume** | Low              | Low         | Low      | High       |

Even though **custom** is the most obvious style for **general-purpose circuits** and **semicustom** is the one for **application-specific circuits**, **processor implementations** have been done in **semicustom** and **ASIC** in **custom**, as in the case of some **space applications**.

{% hint style="danger" %}
It is important to stress that **ASIC** is **not a synonym** for **semicustom style**, as often **erroneously considered**.
{% endhint %}
