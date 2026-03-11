# Lab 04 - High Level Synthesis

In Lab 04, we will have two parts to do/demo.

1. HLS: This part is basically to replace the HDL `myip` we've designed in Lab 01 with the HLS version  along with different optmization skills used in HLS.
2. PYNQ.

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
