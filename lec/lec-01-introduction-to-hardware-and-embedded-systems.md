# Lec 01 - Introduction to Hardware and Embedded Systems

## Embedded Systems Overview

Unlike general-purpose computers (desktops/laptops), an **Embedded System** is a computer system embedded within a larger device, typically dedicated to a specific task.

* **Ubiquity**: There are approximately 50+ devices per household (e.g., washing machines, routers), vastly outnumbering general-purpose computers.
* **Examples**: Rocket engine controls, telecom switches, ABS in cars, set-top boxes, and smart devices .

Embedded systems have the following characteristics:

* **Dedicated Task**: Usually performs a single function or a specific set of functions.
* **Real-Time Response**: Must monitor the environment and react within a specified time frame (hard or soft real-time constraints).
* **Continuous Operation**: Often requires $$ $24 \times 7$ $$24x7 operation with high reliability.
* **Constraints**: heavily constrained by power (battery life), cost, size (form factor), and legacy support.
* **HW/SW Integration**: Tight integration between Software (flexible, sequential) and Hardware (rigid, concurrent).

### Architecture

An embedded system usually consists of a CPU, memory (SRAM/DRAM/Flash), timers, and communication interfaces (I2C, SPI, UART) connected via a system bus as shown below.

<figure><img src="../.gitbook/assets/embed-system-architecture.png" alt=""><figcaption></figcaption></figure>

## Design Considerations & Trends

### System Design Considerations

When designing an embedded system (sensor -> processor -> actuator), we must balance conflicting requirements:

* **Time-to-market**: How fast can we ship?.
* **Technology & Cost**: Availability of IP cores, CAD tools, and chip area .
* **Performance vs. Power**: High performance usually dictates higher power, but embedded devices are often battery-operated.

### Current Trends

{% hint style="info" %}
**Note:** The history of hardware (Moore's Law, Power trends, and the move toward heterogeneous systems) is covered in detail in the [CG3207 Lec 01](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-01-history-technology-performance#history). (From [History](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-01-history-technology-performance#history) to [Todos](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-01-history-technology-performance#todos))&#x20;
{% endhint %}

## Hardware Design Flow

{% hint style="info" %}
**Note:** The abstraction levels (Gate, Circuit, Layout) and the general concept of RTL are discussed in [CG3207 Lec 02 Notes on Digital System Design](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-02-digital-system-design-and-verilog#digital-system-design). However, EE4218 adds the Hardware/Software codesign part.
{% endhint %}

### HW/SW Co-design

The goal is to find the optimal point on the Pareto Curve (trading off Resources vs. Performance)

<figure><img src="../.gitbook/assets/hw-sw-codesign-pareto-curve.png" alt="" width="331"><figcaption></figcaption></figure>

* **Partitioning**: Deciding which parts of the system run on software (CPU) and which run on hardware (ASIC/FPGA).
* **Co-simulation**: Verifying both HW and SW together.

<figure><img src="../.gitbook/assets/hw-sw-codesign-steps.png" alt="" width="375"><figcaption></figcaption></figure>

## FPGA

{% hint style="info" %}
**Note**: The working principles of FPGA is covered in [Harris & Harris DDCA](https://app.gitbook.com/o/MnEKr5A4lYXtOfhoXGj5/s/jTJFBPtKk6NwweAooH53/)!
{% endhint %}

## Algorithms and Graphs

In this course (EE4218), we focus on the algorithms _inside_ the CAD tools. Most CAD problems (like placement and routing) are **NP-hard**.

### Complexity

We measure algorithms using [**Big-O notation**](https://app.gitbook.com/o/MnEKr5A4lYXtOfhoXGj5/s/KipySCGxC8NC1UpA24DS/) ($$O(f(n))$$), which ignores constants and describes the trend as input size $$n$$ grows.

* **P (Polynomial)**: Solvable in polynomial time (Tractable).
* **NP (Non-deterministic Polynomial)**: Exact solution requires exponential time. We usually settle for "heuristics" to find a "good enough" solution rather than the optimal one.

### Graphs

CAD tools represent circuits as graphs.

Graph $$G=(V, E)$$: A set of vertices (nodes) $$V$$ and edges (links) $$E$$.

* **Directed**: $$e_k = (v_i, v_j)$$ (Ordered pair).
* **Undirected**: $$e_k = \{v_i, v_j\}$$ (Unordered pair).
* **Hypergraph**: An edge can connect _more_ than two vertices (common in netlists where one output drives multiple inputs).

#### Graph Representation

We can use the following two methods to represent a graph:

* **Adjacency Matrix**: A 2D array where $$A[i][j] = 1$$ if an edge exists. Good for dense graphs, but uses $$O(|V|^2)$$ memory.
* **Adjacency List**: An array of linked lists. More memory efficient for sparse graphs (like circuits).

#### Common Algorithms in EDA

* **Depth-First Search (DFS)**: Visits nodes by going as deep as possible before backtracking. Used for connectivity checks.
* **Dijkstra**: Shortest path (used in Routing).
* **Graph Coloring**: Used for Register Allocation.
* **Min-Cut**: Used for Partitioning logic into different blocks.
