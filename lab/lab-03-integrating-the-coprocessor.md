# Lab 03 - Integrating the Coprocessor

## System Architecture

With the help of Lab 01 and Lab 02, our understanding of the system architecture becomes more and more mature. At this point of time, we are going to see the complete system architecture of the system we are designing in EE4218! In this lab, we are going to see two architectures

* FIFO implementation
* DAM implementation

### AXI-Stream FIFO

To understand the AXI-Stream FIFO architecture in depth, we are going to look at what's actually happening during the following two processes:

1. Laptop/Terminal program sends data to the coprocessor
2. Laptop/Terminal program receives data from the coprocessor

#### Send Data

The laptop sending data process can be illustrated as follows:

<figure><img src="../.gitbook/assets/lab03-sysmte-architecture-send-data.svg" alt=""><figcaption></figcaption></figure>

The detailed steps are as follows:

1. Laptop/Terminal program sends the data (in our case it's in `.csv` form) through the USB2Serial cable (a.k.a, via UART) to the UART peripheral on the PS.
2. The UART peripheral will put the data received in a small **buffer** first.
3. The processor (ARM A53) will fetch the data from the UART peripheral's buffer and then store it into an array (in our case, it's called `SourceBuffer`) on the DDR memory.
4. Then, the processor will fetch the data from the DDR memory (the `SourceBuffer`) and send it to the TX Buffer on the AXI-Stream FIFO.
5. The AXI-Stream FIFO will then send the data in the TX Buffer to the Coprocessor.

In this process, we have cleared some very important confusions from Lab 01 and Lab 02.

{% stepper %}
{% step %}
#### Confusion 1: What on earth is AXI Bus used for?

In our system, the peripherals like UART, DDR Controller, AXI-Stream FIFO and AXI-Timer are all **MMIO** peripherals. **AXI Bus** can be just thought of as a special connection to read and write to a **"memory" block**. Thus, we may notice that in Lab 02, each of the **MMIO peripheral** has its own **address**. Thus, only the MMIO peripherals connected via the AXI bus can be seen and accessed by the A53 processor directly by using `lw`/`sw` instruction (or `LDR`/`SDR` in ARM Assembly).
{% endstep %}

{% step %}
#### Confusion 2: How to quickly decide the direction of `M_AXIS_xx` and `S_AXIS_xx`?

In Lab 01, the direction of the `M_AXIS_XXX` and `S_AXIS_XXXX` always makes us confused and this is also one question being asked during the lab demonstration. Today, at this point of time, hopefully we can understand it thoroughly by reading the following explanation! First, the AXIS interface always has two ends, namely:

1. Master
2. Slave

From the perspective of the coprocessor, `S_AXIS_xxx` is used to indicate the **status** of the coprocessor when data is being **transmitted** to it. Usually, `S_AXIS_xxx` is used for **data to come in**, except for the `S_AXIS_TREADY`, which is used to indicate that the slave device, which is the coprocessor, is **ready** to receive the data now.

Similarly, from the same perspective of the coprocessor, the `M_AXIS_xxx` is used to indicate status of the coprocessor when data is being **sent** out from it. Usually, the `M_AXIS_xxx` is used for **data to come out**, except for the `M_AXIS_TREADY`, which is used to indicate that the slave device, which is the testbench, is **ready** to receive the data now.

In short, the input/output information of these signals can be seen from below:

{% code overflow="wrap" lineNumbers="true" %}
```verilog
// slave in interface
output reg S_AXIS_TREADY;       // Ready to accept data in
input [31:0] S_AXIS_TDATA;      // Data in
input S_AXIS_TLAST;             // Optional data in qualifier
input S_AXIS_TVALID;            // Data in is valid

// master out interface
output reg M_AXIS_TVALID;       // Data out is valid
output reg [31:0] M_AXIS_TDATA; // Data Out
output reg M_AXIS_TLAST;        // Optional data out qualifier
input M_AXIS_TREADY;            // Connected slave device is ready to accept data out
```
{% endcode %}
{% endstep %}
{% endstepper %}

#### Receive Data

Similarly, the process of the laptop receiving data from the coprocessor can be illustrated as follows:

<figure><img src="../.gitbook/assets/lab03-sysmte-architecture-receive-data.svg" alt=""><figcaption></figcaption></figure>

The detailed steps are as follows:

1. The coprocessor sends the results via AXIS to the AXI-Stream FIFO.
2. The data is stored in the RX buffer on the AXI-Stream FIFO first.
3. The processor fetches the data from the RX buffer on the AXI-Stream FIFO and stores it into an array on the DDR memory (in our case, it's called `DestinationBuffer`).
4. Then the processor will fetch the data in the array and send it to the UART peripheral.
5. The data is stored in the buffer in the UART peripheral for a short while and then it is sent to the Laptop.

### AXI DMA

In the AXI-Stream FIFO architecture, one big disadvantage is the **large overhead**. This is because in this architecture, the processor sends the data **byte-by-byte** from the DDR Memory to the AXI-Stream FIFO and the FIFO won't send the data to the corprocessor until its TX buffer is full. Another disadvantage is the AXI-Stream FIFO **size**, which depends on the size of the data[^1]. To optimize the design, we will use the AXI DMA to replace the AXI-Stream FIFO and its architecture can be shown as follows:

<figure><img src="../.gitbook/assets/lab03-system-architecture-axi-dma.svg" alt=""><figcaption></figcaption></figure>

In this architecture, we have made the following changes

1. **AXI DMA** is not just a **slave** like the AXI-Stream FIFO, it can act as a **master** to control the DDR Controller peripheral on the PS.
2. The two master interfaces added in the AXI DMA are used for **reading** and **writing** data from/to the DDR memory. They have their specific names also:
   1. For the read-from-DDR AXI master interface, it is called **MM2S**, which stands for **memory map to stream**.
   2. For the write-to-DDR AXI master interface, it is called **S2MM**, which stands for **stream to memory map**.

{% hint style="warning" %}
These master AXI interfaces are connected to a **smartconnect** and then this **smartconnect** will be connected via AXI bus to the DDR Controller peripherial.
{% endhint %}

Now we can look at how the DMA works and some "issues" with it.

#### Send Data

The process of the laptop sending the data can be illustrated as follows:

<figure><img src="../.gitbook/assets/lab03-system-architecture-dma-send-data.svg" alt=""><figcaption></figcaption></figure>

The steps in detail are:

1. Same as the AXI-Stream FIFO [sending data process](lab-03-integrating-the-coprocessor.md#send-data), the data will go into the `SourceBuffer` in the DDR Memory first.
2. Now, instead of the processor does the data moving, the processor will send **4 things** to the AXI DMA, informing it what to do. These 4 things are:
   1. The **start address** of the `SourceBuffer` (TX Buffer in the DDR Memory) and `DestinationBuffer` (RX Buffer in the DDR Memory)
   2. The **size** of the data to be **transmitted to the coprocessor** and **received from the coprocessor.**
3. Then the AXI DMA will take over the control from the processor and move the data from `SourceBuffer` via its **read interface** to the coprocessor via AXIS.

#### Receive Data

Similarly, the process of the laptop receiving data can be illustrated as follows:

<figure><img src="../.gitbook/assets/lab03-system-architecture-dma-receive-data.svg" alt=""><figcaption></figcaption></figure>

The steps in detail are:

1. As we have already specified the **start address** and the **size** of the `DestinationBuffer` on the DDR Memory. The DMA will receive the results via AXIS first and then transfer the result directly to the DDR Memory through its **write interface** via AXI Bus.
2. After the `DestinationBuffer`  is filled, the processor will send the data from the `DestinationBuffer` to the laptop, which is similar to what we have seen in the [receive data](lab-03-integrating-the-coprocessor.md#receive-data) process in the AXI-Stream FIFO architecture.

#### Issues

In the [AXI-Stream FIFO architecture](lab-03-integrating-the-coprocessor.md#axi-stream-fifo), it's not hard to find out that all the data that goes into or taken out from the DDR memory will pass through the **cache**. So, there is no issue with the data consistency. However, the case becomes a bit trickier when we move to the [AXI DMA design](lab-03-integrating-the-coprocessor.md#axi-dma) and this will cause some issues in both the send and receive data stage.

{% stepper %}
{% step %}
#### Send Data

In our system, the **data cache** uses the **write-back** mechanism, meaning that the update initially happens at the cache and these updates won't be reflected to the DDR memory until either the cache is full or depending on the replacement policy used in the cache. This will create the **problem** that:

> when the AXI DMA moves data from `SourceBuffer` to the coprocessor, the data in the `SourceBuffer` address in the DDR memory **might not be** the most updated data!

To fix this issue, we will use **cache flush**, which is to force to flush the cache block that contains the `SourceBuffer` address range to the DDR memory so that the DDR memory will have the most updated data. This cache flush should happen exactly **after** the processor finishes writing the data to the `SourceBuffer` and **before** the AXI DMA starts to move the data to the coprocessor.
{% endstep %}

{% step %}
#### Receive Data

The issues don't stop here. In the [receive data stage](lab-03-integrating-the-coprocessor.md#receive-data-1), remember that the last step is the processor moving data from the `DestinationBuffer` to the UART peripheral. In this step, the **problem** is that

> the address of the `DestinationBuffer` might have already been **cached** in the data cache, causing the fact that the processor won't send the most updated data at the `DestinationBuffer` in the DDR memory to the UART peripheral. Instead, the stale and out-dated cached data will be sent! This is troublesome.

To solve this issue, we must use **cache invalidate**, which is to invalidate that specific cache block containing the `DestinationBuffer` address range. This will force the cache to fetch from the DDR memory when the processor wants to access the data at the `DestinationBuffer` address. However, the **problem** doesn't stop here:

> The **cache block** that is **invalidated** might not contain only the data in the `DestinationBuffer`, it may contain some other variables which might not be **reflected** to the DDR memory yet. Invalidating these variables means that we are losing their value!

To solve this issue, during the [send data cache flush](https://wenbo-notes.gitbook.io/ee4218-hsd-notes/lab/lab-03-integrating-the-coprocessor#send-data-2), we must also **flush** the cache block containing the `DestinationBuffer` address range so that the changes to other variables can be **reflected** to the DDR memory.
{% endstep %}
{% endstepper %}

In summary, the flow is shown as follows:

1. In the **send data** stage: The processor writes to the `SourceBuffer` -> Flush the cache block containing both the `SourceBuffer` and `DestinationBuffer` address range -> The processor initiates the AXI DMA transfer.
2. In the **receive data** stage: The processor initiates the DMA write to DDR memory -> The processor **invalidates** the cache block containing the `DestinationBuffer` address range -> The processor then reads the data in the `DestinationBuffer` and sends it to the laptop.

#### Food for Thought

This part contains some food for thoughts that might be beneficial for your understanding of the AXI DMA.

{% stepper %}
{% step %}
#### Why the AXI DMA doesn't need a big buffer like AXI-Stream FIFO?

In the AXI-Stream FIFO implementation, the data movement from the DDR to the AXI-Stream FIFO is done by the **processor**. In other words, the AXI-Stream FIFO cannot read the data directly from the DDR memory, it needs a "bridge", which is the processor. The processor needs to put the data together into a "large" buffer in the AXI-Stream FIFO and then the AXI-Stream FIFO can send the data block to the coprocessor.

In the AXI-DMA implementation, the AXI-DMA module can read the data in the DDR memory directly and send it to via AXIS to the coprocessor. Thus, there is no need to have a "large" buffer which is dedicated to storing the data to be sent to the coprocessor.
{% endstep %}

{% step %}
#### Why AXI DMA uses AXI-Lite while AXI-Stream FIFO uses AXI-FULL?

In AXI-DMA implementation, the processor only needs to tell the AXI-DMA some **configuration information**, like the start address of the `SourceBuffer` and `DestinationBuffer`. The heavy data transmission is done by the DMA's master interfaces (MM2S/S2MM), which **do** use full AXI to talk to the DDR.

In the AXI-Stream FIFO implementation, AXI-FULL is used because the processor uses this interface to send the actual bulk of data to the AXI-Stream FIFO. One important difference between AXI-Lite and AXI-Full is that AXI-Full supports **burst mode** for data transmission. Thus, the usage of AXI-Full on the AXI-Stream FIFO allows the processor to blast chunks of data into the FIFO much faster.
{% endstep %}
{% endstepper %}

## Performance Comparison

One important thing to do in Lab 03 is to compare the performance between three applications:

1. Pure software + no data transmission
2. Coprocessor + FIFO data transmission
3. Coprocessor + DMA data transmission

Given that, what my group gets at this point of time is shown as a table below.

{% hint style="success" %}
The clock frequency is 100MHz on our system.
{% endhint %}

| Pure software | Coprocessor + FIFO | Coprocessor + DMA |
| ------------- | ------------------ | ----------------- |
| 1392 cycles   | 50025 cycles       | 1817 cycles       |

From this table, we might wonder why there is no really impressive performance improvement from the coprocessor comparing to the pure software is because our current application, which is a matrix multiplication of a 64x8 and 8x1 matrix, is not **arithmetic intense**.

<details>

<summary>What is <strong>Arithemetic Intensity</strong>?</summary>

**Arithmetic Intensity (AI)** is a measure of how much calculating we do compared to how much data we have to move around to do it. It is typically measured in **Operations per Byte** (Ops/Byte) or **FLOPs per Byte**. The formula for AI is

<p align="center"><span class="math">\text{AI} = \frac{\text{Total Arithmetic Operations}}{\text{Total Memory Traffic (Bytes)}}</span></p>



* **High Arithmetic Intensity** **(Compute-Bound):** We do a lot of math on a small amount of data. This is where coprocessors and GPUs shine because they can chew through the math while the data just sits in their local memory.
* **Low Arithmetic Intensity (Memory-Bound)**: We do very little math per piece of data. The processor spends most of its time waiting for data to be transferred rather than actually computing.

So, why in lab 03, our matrix multiplication has Low Arithmetic Intensity? Let's calculate the total arithmetic operations and the total memory traffic in detail:

1. **Total Arithmetic Operations**: Each row of matrix A (64x8) is multiplied by the only column in matrix B (8x1). So, for each element in the result matrix's, we are doing 8 multiplications and 7 additions. Thus, for the total 64 elements in the result matrix, we are doing $$64\times15=960$$ operations.
2. **Total Memory Traffic**: As we have $$64\times8+8\times1+64\times1=584$$ elements to transfer and each element currently takes **4 bytes** in our application. Thus, our **total memory traffic** in bytes is $$584\times4=2336$$.

Thus, the AI for our application is $$960\div 2336\approx0.41$$. This low arithemtic intensity is the main reason that we don't really see the performance improvement between the pure software and the coprocessor implementation.

{% hint style="success" %}
#### How does pipelining affect the total arithmetic operations?

The answer is the **pipelining** used in our coprocessor doesn't affect the total arithmetic operations. As the number of the operations doesn't change but the execution time for completing one matrix multiplication might change. Another angle is that Arithmetic Intensity refers to the the **application**, which is the matrix multiplication in our case, it doesn't care about how this application is implemented internally.
{% endhint %}

</details>

## Reference

The implementation of the DMA code utilizes the overall structure from Lab 02 and modifies according to the DMA Polling example from AMD.

## Reference

1. [AXI Basics on Youtube](https://youtube.com/playlist?list=PLkqJVNOiuuHtNrVaNK4O1BSgczja4obeW\&si=sSgWNK6TMGORjrlF).

[^1]: In our lab context, the data to be sent to the coprocessor is the matrix A and B while the data to be received from the coprocessor is the result matrix.
