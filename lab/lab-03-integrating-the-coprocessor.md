# Lab 03 - Integrating the Coprocessor

## System Architecture

With the help of Lab 01 and Lab 02, our understanding of the system architecture become more and more mature. At this point of time, we are going to see the complete system architecture of the system we are designing in EE4218! In this lab, we are going to see two architectures

* FIFO implementation
* DAM implementation

### AXI-Stream FIFO Architecture

To understand the AXI-Stream FIFO architecture in depth, we are going to look at what's actually happening during the following two processes:

1. Laptop/Terminal program sends data to the coprocessor
2. Laptop/Terminal program receives data from the coprocessor

#### Send Data

The laptop sending data process can be illustrated as follows:

<figure><img src="../.gitbook/assets/lab03-sysmte-architecture-send-data.svg" alt=""><figcaption></figcaption></figure>

The detailed steps are as follows:

1. Laptop/Terminal program sends the data (in our case it's in `.csv` form) through the USB2Serial cable (a.k.a, via UART) to the UART peripheral on the PS.
2. The UART peripheral will put the data received in a smaller **buffer** first.
3. The processor (ARM A53) will fetch the data from the UART peripheral's buffer and then store it into an array (in our case, it's called `SourceBuffer`) on the DDR memory.
4. Then, the processor will fetch the data from the DDR memory (the `SourceBuffer`) and send it to the TX Buffer on the AXI-Stream FIFO.
5. The AXI-Stream FIFO will then send the data in the TX Buffer to the Coprocessor.

In this process, we have cleared a very important confusion from Lab 01 and Lab 02, which is "what on earth is AXI Bus used for?".

In our system, the peripherals like UART, DDR Controller, AXI-Stream FIFO and AXI-Timer are all **MMIO** peripherals. **AXI Bus** can be just thought of as a special connection to read and write to a **"memory" block**. Thus, we may notice that in Lab 02, each of the **MMIO peripheral** has its own **address**. Thus, only the MMIO peripherals connected via the AXI bus can be seen and accessed by the A53 processer directly by using `lw`/`sw` instruction (or `LDR`/`SDR` in ARM convention).

#### Receive Data

Similarly, the process of the laptop receiving data from the coprocessor can be illustrated as follows:

<figure><img src="../.gitbook/assets/lab03-sysmte-architecture-receive-data.svg" alt=""><figcaption></figcaption></figure>

The detailed steps are as follows:

1. The coprocessor sends the results via AXIS to the AXI-Stream FIFO.
2. The data is stored in the RX buffer on the AXI-Stream FIFO first.
3. The processor fetches the data from the RX buffer on the AXI-Stream FIFO and store it into an array on the DDR memory (in our case, it's called `DestinationBuffer`).
4. Then the processor will fetch the data in the array and send it to the UART peripheral.
5. The data is stored in the buffer in the UART peripheral for a short while and then it is sent to the Laptop.

## Reference

1. [AXI Basics on Youtube](https://youtube.com/playlist?list=PLkqJVNOiuuHtNrVaNK4O1BSgczja4obeW\&si=sSgWNK6TMGORjrlF).
