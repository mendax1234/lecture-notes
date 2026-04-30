# Quiz 2

## Problems

### 1. Boolean Match

**Quesiton:** It is possible for two circuits that match structurally to have a boolean signature mismatch.

* [ ] True
* [x] False

***

**Sol.** The answer is obviously **false**. But, boolean mismatch means the two functions mismatch.

### 2. AXI Stream FIFO

**Question**: The AXI Stream FIFO IP does NOT have an AXI Master interface capable of accessing the system memory (DDR RAM) directly.

* [x] True
* [ ] False

***

**Sol.** The answer is **true**. The AXI Stream FIFO is pure slave, but the AXI DMA has a master interface.

### 3. LUT

**Question**: The number of 3-input LUTs required in order to be able to implement any 4-input combinational logic without requiring any other type of components is \_\_\_\_\_ (enter a number).

***

**Sol.** The answer is **3**. Based on the lectures, any 4-input combinational logic can be implemented by a **two** 3-input LUTs plus a 2-to-1 multiplexer. As there is no multiplexer available here, we should use a third 3-input LUT to implement the 2-to-1 multiplexer. Below is an illustration for the implementation.

<figure><img src="../.gitbook/assets/quiz2-lut.svg" alt=""><figcaption></figcaption></figure>

And to use a 3-input LUT to implement a 2-to-1 multiplexer and the last 3-input LUT entries are shown in the following table:

| a | b | x3 | y |
| - | - | -- | - |
| 0 | 0 | 0  | 0 |
| 0 | 0 | 1  | 0 |
| 0 | 1 | 0  | 0 |
| 0 | 1 | 1  | 1 |
| 1 | 0 | 0  | 1 |
| 1 | 0 | 1  | 0 |
| 1 | 1 | 0  | 1 |
| 1 | 1 | 1  | 1 |

Basically, `x3` is the select signal, and when `x3==0`, the output `y=a`. Otherwise, the output `y=b`. To fit into the LUT that we have learned in the [lecture](../lec/lec-08-technology-mapping.md#lut-basics), this configuration memory for the 3-input LUT will be the column `y` and the three select signals will be `a, b, x3`.

### 4. Timing

**Quesiton**: For a particular path, the following information is derived from the timing report after place and route:

* Maximum combinational delay: 5 ns
* Clock-Q propagation delay: 2 ns
* Clock period: 10 ns
* Clock skew: 2 ns
* Clock jitter and other uncertainties: 1 ns
* Setup time: 2 ns

Suppose you have the means to change the clock frequency. What is the maximum frequency at which the circuit can run reliably (disregarding the timing of other paths, if any)?

***

**Sol**. This is a classic timing question and if you have taken EE4415 and understand Lec 07 well, it should be a piece of cake for you! As this is a setup time question, we can calculate the data arrival time and the data required time:

1. Data arrival time: $$t_{\text{CK-Q}}+t_{\text{comb,max}}=5+2=7$$
2. Data required time: $$t_{\text{CLK}}-t_{\text{setup}}+t_{\text{skew}}-t_{\text{jitter}}=t-2+2-1=t-1$$

As the data arrival time should be smaller than the data required time, solve it, we have

$$
7\leq t-1
$$

Thus, the minimum of clock period $$t$$ is 8ns. Thus, the clock frequency is 125MHz.

### 5. Retiming

This question will be very easy if you have taken EE4415! I will skip here.
