# Studio 15 - Secure Networking

## TCP/IP

The TCP/IP protocol includes a series of protocols, also known as the TCP/IP [protocol suite](studio-13-communication-protocol.md#protocol-suite-stack). It provides a point-to-point connection mechanism and standardizes the encapsulation, addressing, transmission, routing, and reception of data frames.

#### TCP/IP vs. OSI

Unlike OSI Model, which has 7 layers, TCP/IP usually has four layers:

1. Application
2. Transport
3. Network
4. Network Interface: Sometimes this layer is divided into two layers
   1. Physical Layer
   2. Link Layer

<figure><img src="../.gitbook/assets/studio15-tcpip-vs-osi.png" alt="" width="375"><figcaption></figcaption></figure>

### Application Layer

The **Application Layer** of the TCP/IP protocol handles protocols that enable communication between applications. For example, **HTTP** (Hypertext Transfer Protocol) is used for web browsing, allowing clients to request web pages from servers and receive the corresponding data.

### Transport Layer

The **Transport Layer** of the TCP/IP protocol handles end-to-end communication and ensures **reliable data delivery** between devices. Its main functions include:

1. Providing transport services for end-to-end connections.
2. Offering both reliable (TCP) and unreliable (UDP) transport services.
3. Managing flow control, error control, and Quality of Service (QoS).

There are two main protocols in the Transport Layer:

* **TCP** (Transmission Control Protocol) is a connection-oriented, **reliable protocol** that ensures data arrives intact, in order, and without errors. It provides flow control and congestion management to avoid network overload and guarantees data delivery.
* **UDP** (User Datagram Protocol) is a connectionless, unreliable protocol. It does not guarantee delivery or order, making it **faster but less reliable**. It's typically used for applications where speed is critical and some data loss is acceptable (e.g., video streaming or chat applications).

In summary, **TCP** offers **high reliability but lower efficiency**, while **UDP** offers **higher efficiency but lower reliability,** making it suitable for real-time applications with less stringent reliability requirements.

### Network Layer

The **Network Layer** of the TCP/IP protocol is responsible for **finding an appropriate path for transmitting data packets** across complex network environments. In simple terms, the Network Layer ensures that **data is delivered to the correct destination address**, which may be connected through multiple networks and routers.

Key protocols at the Network Layer include:

* **ICMP** (Internet Control Message Protocol)
* **IP** (Internet Protocol)

These protocols help with routing, error reporting, and managing network traffic between different networks.

### Network Interface Layer

The Network Interface Layer, which includes the **Link Layer** and **Physical Layer**, handles the hardware connections in a network. This layer includes device drivers, Network Interface Cards (NICs), optical fibers, and all physical transmission media. At this layer, the unit of data transmission is a **bit**.

Key protocols at the Link Layer include:

* **ARP** (Address Resolution Protocol)
* **RARP** (Reverse Address Resolution Protocol)

These protocols are responsible for mapping IP addresses to MAC addresses and vice versa, enabling devices on a local network to communicate effectively.

### Example

When using TCP/IP for network communication, data packets are transmitted according to the layered model.

* The sender transmits from the **application layer downwards**,&#x20;
* the receiver processes from the **network interface layer upwards**.

The order of data transmission for each frame from the client to the server is as follows: Application Layer → Transport Layer → Network Layer → Network Interface Layer → Network Interface Layer → Network Layer → Transport Layer → Application Layer.

<figure><img src="../.gitbook/assets/studio15-tcp-ip-http-example.png" alt="" width="560"><figcaption></figcaption></figure>

#### **Data Encapsulation and Decapsulation**

When data is transmitted over the internet, it cannot be sent without any identifiers, as that would lead to chaos.

* Therefore, when sending data, specific identifiers need to be added. This process is known as data **encapsulation**.&#x20;
* When the data is used, these identifiers are removed, and this process is called **decapsulation**.

The data encapsulation and decapsulation process in the TCP/IP protocol is generally as shown in the figure below:

<figure><img src="../.gitbook/assets/studio15-tcpip-encapsulation.png" alt="" width="498"><figcaption></figcaption></figure>
