# Revision Problems

## Problems

In the last two lectures, prof. Rajesh guided us to do some very insightful pyp questions, which I think are quite useful for the finals in this sem.

1. Resource sharing will **mess up** the microarchitecture of pipelining. Also, in pipelining, we assume we use dedicated binding, thus both our functional resources usage and the register usage will blow up. However, in the real-case, we assume that we have enough of these resources, thus, doing the pipeline way is preferable and is also commonly used.
2. The sequence detector (AY19/20 PYP) is a very classic application of FSM.
3. Search and see how Prof uses the word "manifest"
4. Boolean match is equal to function match. Boolean signature match is the signature match we've been talking about.
5. Usually, there is an IP block called asynchronous FIFO which will take care of metastability filtering etc.
6. HLS is good at resource-dominated circuits (like DSPs) but not good at non-resource dominated circuits (like microprocessors)
7. If there is hold time violation only at one core, we can disable that core and still sell that chip as lower model, like i3, etc.
8. Include the discrete maths formula, liek $$\bar ab+ac=a+bc$$, etc.
9. In the Verilog synthesis tool, the manipulation (addtion/multiplication) of the constants (`localparam`) are done by the compiler so no hardware will be needed for it.
10. In the HLS, the true dual port is automatically enabled unless the question says we are using the single-port memory.
11. In the HLS II analysis, the II is limited by the **slowest stage**, so if in one operation, we need to read 3 data without using array partitioning, we needs two cycles to read the data and thus our II will be 2. But if let's say our multiplier takes 2 cycles to calculate, the II will still be 2! Only when the execute stage takes more than 3 cycles, then our II will be 3. In summary, the II will be the number of cycles taken by the slowest macro stage (read, execute, etc).

## Tips

Things to print out:

1. [Vivado HLS Optimization Guide](https://docs.amd.com/v/u/en-US/ug1270-vivado-hls-opt-methodology-guide). (Page 83, Page 91, Page 116, Page 125 are very very clear and useful!)
2. Lecture 7 slides.
3. EE4415 [Lec1b-Timing Synchronous](https://app.gitbook.com/s/Sp0XaarBjbEX3JIMrRaR/part-1-lec-digital-design-flow/lec-1b-timing-synchronous).
4. Print the unit table, which will be useful for the clock frequency.
