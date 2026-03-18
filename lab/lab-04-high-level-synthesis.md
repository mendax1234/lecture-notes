# Lab 04 - High Level Synthesis

In Lab 04, we will have two parts to do/demo.

1. HLS: This part is basically to replace the HDL `myip` we've designed in Lab 01 with the HLS version  along with different optmization skills used in HLS.
2. PYNQ: This is an alternative to replace the **standalone** PS. PYNQ gives us a full Linux OS and python library to operate on. Given that, many fancy stuff like network, image processing, etc are supported on PYNQ. The trade-off is that the **overhead** will be a lot!

## HLS

In this part, we will need to compare the performance improvement between the different versions of `myip` generated used Vitis HLS.

### Connect 2 Coprocessors

In the lab munual, Prof Rajesh said that it is recommended to connect 2 coprocessors to our Zynq so that we can demo the performance difference between the baseline HLS version with the optimized HLS version quickly. To do this on DMA, the complete block diagram is shown below.

<figure><img src="../.gitbook/assets/connect-2-coprocessors.png" alt=""><figcaption></figcaption></figure>

To do so, we need to make the following changes:

1. Add another AXI-DMA and the optimized IP, make the connection between `M_AXIS_MM2S` (on DMA) and `S_AXIS` (on coprocessor) and between `S_AXIS_S2MM` (on DMA) and `M_AXIS` (on coprocessor).
2. Increase the number of master interfaces on `axi_smc` from 2 to 3.
   1. Connect the third master interface `M02_AXI` to the `S_AXI_LITE` on the newly added DMA.
3. Increase the number of slave interfaces on `axi_sma_1` from 2 to 4.
   1. Connect the third slave inteface `S02_AXI` to the `M_AXI_MM2S` on the newly added DMA.
   2. Connect the fourth slave inteface `S03_AXI` to the `M_AXI_S2MM` on the newly added DMA.
4. Run the automation to connect the Reset and Clock pins.

### HLS Baseline

As my Vitis (2025.2) will do some optimizations automatically, to disable these optimizations, we need to change the `hls.syn.compile.pipeline_loops` to 0 in the `hls_config.cfg`. Besides, it is safer for us to add the `#pragma HLS pipeline off` manually in our code as well. After doing all this, to verify that our HLS doesn't have any optimization, we can see from the "Performance & Resource Estimates" in the C synthesis report.

<figure><img src="../.gitbook/assets/c-synthesis-report-baseline.png" alt=""><figcaption></figcaption></figure>

More specifically, the "pipelined" column will show **no** for all the loops. This will indicate that our baseline is a **real** baseline.

{% hint style="warning" %}
After we make any changes to the HLS IP, we need to upgrade the IP in the block diagram in our Vivado. After that, we need to generate new bitstream and update the platform on our Vitis by using the "Switch/Re-read XSA" in the `vitis-comp.json` so that we don't need to create new platform.
{% endhint %}

### HLS Optimization

The purpose of this lab is to demonstrate **one and only one** HLS optimization. Thus, based on our application, which is the matrix multiplication, we have enough resources and thus **unroll** will give us the best performance but worst area usage.

#### Understand HLS Optimization



#### Unrolling

More specifically, we **fully unroll** all the 4 loops in our application. Theoretically, the clock cycles taken can be divided into three parts

1. Read input A and B: 512 + 8 = 520 cycles.
2. Compute: 1 (if FSM is used to implement, maybe have 1 or 2 more clock cycles)
3. Write output: 64 cycles.

And the real number of clock cycles we get is 587, which is around the same as the theoretical and the 2 more cycles indeed come from the FSM state transition within the compute stage.

<figure><img src="../.gitbook/assets/schedule-viewer-lab4.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Doing unrolling without array partitioning is useless. However in 2025.2 version, the array partition can be done automatically by the tool.
{% endhint %}

<details>

<summary>Tips on Unrolling</summary>

1. If the unrolling factor is an integer factor of the maximum iteration count, we can use `skip_exit_check` to skip the exit check and thus minimizing the area and simplifying the logic.
2. `region`: An optional keyword that unrolls all loops within the body (region) of the specified loop, without unrolling the enclosing loop itself.

</details>

#### Interface

In C based design, all input and output operations are performed, in zero time, through formal function arguments. In an RTL design these same input and output operations must be performed through a port in the design interface and typically operate using a specific I/O (inputoutput) protocol.

Here, we use AXIS as our I/O protocol. The data comes into the input and out from the output is done **one by one** because of the following sentence from the Xilinix docs.

> After the block-level protocol has been used to start the operation of the block, the port-level IO protocols are used to **sequence** data into and out of the block.

## PYNQ
