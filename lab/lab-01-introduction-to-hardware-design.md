# Lab 01 - Introduction to Hardware Design

## System Overview

In EE4218, we will be using the Kria KV260 SOM Vision Starter Kit containing a Xilinx Zynq Ultrascale+ SoC to implement a **coprocessor**. The overall system overview can be shown as follows:

<figure><img src="../.gitbook/assets/ultrascale-system-overview.svg" alt=""><figcaption></figcaption></figure>

On our FPGA board, we have two major blocks

1. The Processor System (PS)
2. The UltraScale Programmable Logic (PL)

### Processor System

The processor system of our FPGA board cotains two mains parts

1. The ARM Cortex-A53 In-order Superscalar(dual-issue) processor
2. I/O Peripherals like UART, Ethernet, USB and Debug, etc.

The processor (ARM Cortex-A53) communicates with these I/O peripherals using MMIO[^1] via the **AXI bus**. For example, to communicate, the processor issues standard Load/Store instructions (e.g., `LDR`/`STR` in assembly) targeting these reserved addresses.

### Programmable Logic

The **Ultrascale programmable logic (PL**) again has two very important parts for now

1. The **bridge** which translates the data fetched via AXI into a stream
2. The **coprocessor** which is responsible for doing the matrix multiplication (2x4 times 4x1)

#### Bridge

The **bridge** is responsible for converting AXI transactions into a streaming data format suitable for high-throughput hardware processing. So exacts steps from PS to the bridge are as follows:

1. Laptop sends data via **UART**.
2. The processor uses a `lw` instruction to read the data from the UART register. Then it uses a `sw` instruction to store the data into the **main memory**.
3. The processor uses a `sw` instruction to writes to the **Bridge** modules status register to wake it up. This step will also provide the **Bridge module** with information telling it
   1. **where** to fetch the data from the PS's main memory
   2. **the size** of the data being fetched.
4. The **bridge module** receives the data and converts it into **AXI Stream** for the coprocessor to process.

After the coprocessor done its job, the reverse of the steps above are performed to send the result back to the main memory in the PS.

> TODO: Add AXI vs. AXIS here.

#### Coprocessor

In the coprocessor, we have the following modules

1. 2 RAM: One to store matrix A and the other to store matrix B.
2. The Matrix Multiply Unit: Basically, it is the Multiply-And-Accumulate (MAC) unit.
3. The FSM Control Unit: This is the steering logic within the coprocessor

{% hint style="warning" %}
In Lab 01, we are going to implement the coprocessor and try to optimize its performance.
{% endhint %}

[^1]: The peripherals do not have their own special CPU instructions. Instead, they are mapped to specific physical addresses in the system memory map (hardwired by Xilinx).
