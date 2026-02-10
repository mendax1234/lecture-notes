# Circuit Specifications for Architectural Synthesis

Specifications for architectural synthesis include

1. **behavioral-level circuit models**,
2. **resource descriptions**, and
3. **design constraints**.

Behavioral models are represented using [**sequencing graphs**](../hardware-modeling/abstract-models.md#sequencing-graphs), as described earlier. Here, we focus on resources and constraints in greater detail.

{% hint style="success" %}
The behavioral models can be thought of as the **function** we want to achieve using our circuit.
{% endhint %}

## Resources

Resources implement different hardware functions and can be broadly classified into three categories.

{% hint style="warning" %}
This part can also be called as **building blocks**, which are a set of resources fully characterized in terms of area and execution delay. Basically, we want to know how to connect the **building blocks** we have to achieve the outcome we want.
{% endhint %}

### Functional Resources

**Functional resources** process data by implementing arithmetic or logic operations. They can be further divided into:

1. Primitive resources
2. Application-specific resources

#### Primitive Resources

**Primitive resources**, which are carefully designed reusable subcircuits, such as arithmetic units and standard logic functions (e.g., encoders and decoders).

#### Application-specific Resources

**Application-specific resources**, which implement specialized subtasks. An example is a subcircuit that services a specific interrupt in a processor. These resources are typically implementations of other HDL models.

{% hint style="danger" %}
The major decisions in architectural synthesis are often related to the use of **functional resources**. From the perspective of formulating architectural synthesis and optimization problems, there is no fundamental difference between primitive and application-specific functional resources. Both types can be characterized in terms of area and performance and can be used as building blocks.
{% endhint %}

### Memory Resources

**Memory resources** store data. Examples include registers, read-only memory (ROM), and read-write memory (RAM).

{% hint style="warning" %}
Requirement for storage resources are **implicit** in the **sequencing graph itself**.
{% endhint %}

### Interface Resources

**Interface resources** support data transfer within the system. These include buses, which serve as major communication channels inside a data path.

### Steering Logics

As we have seen [earlier](./), **steering logics** are multiplexers and buses that route data to the correct destination at the appropriate time.

<details>

<summary>Measure the performance of resources in clock-cycle</summary>

When architectural synthesis targets **synchronous circuit implementations**, as is often the case and as considered in this book, it is convenient to measure the **performance** of the **resources** in terms of the **cycles** required to execute the corresponding **operation**, which we call the **execution delay** of the resource.

</details>

## Constraints

Constraints in architectural synthesis can be classified into two major groups: **interface constraints** and **implementation constraints**.

### Interface Constraints

**Interface constraints** are additional **specifications** to ensure that the **circuit** can be embedded in a given **environment**. They relate to the **format** and **timing** of the **I/O data transfers**.

For example, the **timing constraint** is one kind of the **interface constraint**, which includes

* Cycle-time
* Latency of a set of operations
* Time spaing between operation pairs

### Implementation Constraints

**Implementation constraints** reflect the designer’s desire to achieve a **structure** with certain **properties**. Examples include **area constraints** and **performance constraints**, such as **cycle-time** and/or **latency bounds**.

#### Resource Binding Constraints

A different kind of **implementation constraint** is a **resource binding constraint**. In this case, a particular **operation** is required to be implemented by a given **resource**.

{% hint style="success" %}
#### Partial Binding

**Architectural synthesis** with **resource binding constraints** is often referred to as **synthesis from partial structure**.
{% endhint %}

#### Resource Usage Constraints

Sometimes **resource usage constraints** are used in the **synthesis** and **optimization** of **circuits** that are not **resource dominated**.

The example of this constraint is **the number of various building blocks available** (e.g., only 1 adder and 2 multipliers are available).

## Circuit Type

We will see two circuit types in this section, they are

* Resource-dominated circuits
* Non resource-dominated circuits

### Resource-dominated Circuits

In **resource-dominated circuits**, area and performance depend on few, well-characterized functional resources. For example, the adders and multipliers take up most of the available time.

One example of resource-dominated circuits is the DSP circuits.

### Non Resource-dominated Circuits

In **non resource-dominated circuits**, the area and performance are strongly influenced by **sparse logic**, **control** and **wiring**.

Examples are the microprocessors and some ASIC circuits.
