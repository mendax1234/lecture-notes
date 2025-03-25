# Studio 13 - Communication Protocol

## Communication Protocol

Protocols, packets, and communication methods are the invisible glue enabling devices to collaborate. By defining **rules** (protocols), structuring **data** (packets), and choosing **interaction styles** (event-driven or polling), systems like the Arduino-RPi duo achieve precise, reliable control—mirroring how the entire digital world operates.

### Protocols: **The Rules of Engagement**

A **protocol** is a standardized "language" that devices agree to use for interaction. It defines:

* **What** to communicate (e.g., commands, sensor data).
* **How** to format and structure the data.
* **When** and **how** to respond (e.g., acknowledgments, error handling).

#### Protocol Suite/Stack

A protocol suite or stack is a **collection of protocols**, each operating at a **different layer** of the communication process.

For example, in the OSI model, the network communication is divided into seven layers:

* **Physical Layer:** Deals with the hardware transmission of raw bits.
  * e.g. agreement on voltage levels, number of wires to use, how wires are to be connected, etc.
* **Data Link Layer:** Organizes bits into frames and handles error detection.
* **Network Layer:** Manages addressing and routing (similar to IP).
* **Transport Layer:** Provides end-to-end communication services (similar to TCP/UDP).
* **Session Layer:** Manages sessions or connections between applications.
* **Presentation Layer:** Translates data formats and encrypts/decrypts data.
* **Application Layer:** Focuses on _meaningful data exchange_ (e.g., `(move, 50, 1)`).
  * This is where custom protocols (like the Arduino-RPi protocol) operate.

{% hint style="info" %}
Each layer may have its own **protocol**, and the collection of all these 7 layer's protocols is called a **protocol suite/stack**.
{% endhint %}

#### **Why Protocols Matter**

* Without protocols, devices would send raw, unstructured data, leading to misinterpretation and chaos.
* Protocols ensure **interoperability**, **reliability**, and **scalability**.

### **Packets: The Envelopes of Data**

A **packet** is a structured unit of data that adheres to protocol rules. Think of it as a "digital envelope" containing:

* **Header**: Metadata (e.g., sender/receiver IDs, packet type, sequence numbers).
* **Payload**: The actual data (e.g., `move` command, sensor readings).
* **Footer**: Error-checking bits (e.g., checksums) to detect corruption.

#### **Types of Packets in Embedded Systems**

1. **Command Packets**
   * Sent by the RPi to instruct the Arduino.
   * Example: `(move, 50, 1)` → Move at 50% speed for 1 meter.
2. **Acknowledgment (ACK) Packets**
   * Confirm receipt of a command (e.g., `(ack)`).
3. **Data Packets**
   * Carry sensor data (e.g., `(compass, 180)`).
4. **Status Packets**
   * Signal task completion (`(done)`) or errors (`(error, motor_stuck)`).

#### **Key Role of Packets**

* Break data into manageable chunks.
* Enable error detection and retransmission.
* Ensure data arrives in a structured, interpretable format.

### &#x20;**Communication Methods: The Conversation Styles**

How devices initiate and manage data flow depends on the **communication method** used. Two primary approaches exist

#### **Method 1: Event-Driven (Interrupt-Based)**

So, basically it works as follows:

* The Arduino _proactively_ sends data when an event occurs (e.g., task completion, sensor threshold reached).
* Example: Sending a `(done)` packet after moving 1 meter.

| Pros                                                           | Cons                                                 |
| -------------------------------------------------------------- | ---------------------------------------------------- |
| Efficient for time-sensitive data (e.g., collision detection). | Requires robust error handling (e.g., lost packets). |
| Reduces unnecessary network traffic.                           | May overwhelm the RPi if events occur frequently.    |

The following is a demo picture, the LHS is RPI and the RHS is Arduino.

<figure><img src="../.gitbook/assets/studio13-event-driven.png" alt="" width="213"><figcaption></figcaption></figure>

#### **Method 2: Polling (Request-Response)**

So, basically it works as follows,

* The RPi _actively queries_ the Arduino for data/status (e.g., `(poll, compass)`).
* Example: Checking wheel rotations every 2 seconds.

| Pros                                                | Cons                                                     |
| --------------------------------------------------- | -------------------------------------------------------- |
| RPi controls timing and prioritization.             | Higher latency (data is only as fresh as the last poll). |
| Simplifies synchronization in multi-device systems. | Increased overhead with frequent polling.                |

The following is a demo picture, the LHS is RPI and the RHS is Arduino.

<figure><img src="../.gitbook/assets/studio13-poll-driven.png" alt="" width="195"><figcaption></figcaption></figure>

## Transfer Packets

Now, let's take a deeper look at the **packets** and some potential issues that may occur during packet transmission.

### Checksum

**Checksums** are used to check that data is received correctly.

#### Find Checksum

In CG2111A, we use thte **XOR** operation to compute the check sum

```
checksum = b1 XOR b2 XOR ...
```

where, `b1, b2, ...` are the bytes within the packet.

For example, we want to calculate the checksum of `FF EE`

{% stepper %}
{% step %}
**Organize using bytes and change to binary**

So, our packet will become `11111111 11101110`.
{% endstep %}

{% step %}
`XOR` **each bit**

```
    11111111  (current checksum: FF in hex)
XOR 11101110  (b2: EE in hex)
------------
    00010001  (result: 11 in hex)
```

So, the checksum is `11`.
{% endstep %}
{% endstepper %}

#### Verify Checksum

Checksum is usually included in the packet to be sent out. The **receiver** will use the data and the [#find-checksum](studio-13-communication-protocol.md#find-checksum "mention") method to calculate the checksum and compare with the checksum it receives, if they match, we have verified the validity of our data.

### Serialize Structures

To implement a packet, the easiest way is to use strctures. For example,

```c
typedef tc {
    int command;
    int speed;
    int distanceInMeters;
} TCommand;
```

But, when transferring this packet to other devices, we need to **serialize** this packet.

> **Serialization** means converting data into a **platform-agnostic byte stream**, ensuring reliable transmission. This is because when communicating between devices like a **Raspberry Pi (RPi)** and an **Arduino**, data must traverse a **serial interface** (e.g., USB/UART), which only understands **streams of bytes**.

The common steps to do serialize is to:

1. Get a pointer to the structure
2. Copy into an array of char
3. May want to include information on packet length and checksum

With this idea, the following is an example code to **serialize**

{% code lineNumbers="true" %}
```c
unsigned int serialize(char *buf, void *p, size_t size)
{
    char checksum = 0;
    buf[0] = size;
    memcpy(buf + 1, p, size);
    for (int i = 1; i <= size; i++)
    {
        checksum ^= buf[i];
    }
    buf[size + 1] = checksum;
    return size + 2;
}

void sendSerialData(char *buffer, int len)
{
    for (int i = 0; i < len; i++)
    {
        Serial.write(buffer[i]);
    }
}
```
{% endcode %}

<details>

<summary>Why a buffer is needed (or why can't we just use the struct pointer and send bytes from that address?)</summary>

Although you can technically do that. But it will have some issues:

1. Your Raw structs lack headers, checksums, or packet delimiters, making error detection impossible. (a.k.a you need add more fields explicitly in your data structure)

So, TLDR, always serialize the packet into a byte stream, which is stored in buffer, and then send them out.

</details>

### **Deserialize Structures**

Similarly, **deserialize** means converting a stream of bytes back to structures. This can be done using the following steps:

1. Get a pointer to the structure
2. Copy buffer of bytes to that pointer
   1. May need to remove packet length and compute checksum first

For example, the following is a demo code to deserialize.

{% code lineNumbers="true" %}
```c
unsigned int deserialize(void *p, char *buf)
{
    size_t size = buf[0];
    char checksum = 0;

    for (int i = 1; i <= size; i++)
    {
        checksum ^= buf[i];
    }

    if (checksum == buf[size + 1])
    {
        memcpy(p, buf + 1, size);
        return PACKET_OK;
    }
    else
    {
        printf("CHECKSUM ERROR\n");
        return PACKET_BAD_CHECKSUM;
    }
}
```
{% endcode %}
