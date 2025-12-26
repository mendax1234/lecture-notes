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
Note: The history of hardware (Moore's Law, Power trends, and the move toward heterogeneous systems) is covered in detail in the [CG3207 Lec 01](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-01-history-technology-performance#history). (From [History](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-01-history-technology-performance#history) to [Todos](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/lec/lec-01-history-technology-performance#todos))&#x20;
{% endhint %}
