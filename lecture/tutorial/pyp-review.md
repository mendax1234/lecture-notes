# PYP Review

In the last two lectures, prof. Rajesh guided us to do some very insightful pyp questions, which I think are quite useful for the finals in this sem.

1. Resource sharing will **mess up** the microarchitecture of pipelining. Also, in pipelining, we assume we use dedicated binding, thus both our functional resources usage and the register usage will blow up. However, in the real-case, we assume that we have enough of these resources, thus, doing the pipeline way is preferable and is also commonly used.
2. The sequence detector (AY19/20 PYP) is a very classic application of FSM.
3. Search and see how Prof uses the word "manifest"
4. Boolean match is equal to function match. Boolean signature match is the signature match we've been talking about.
5. Usually, there is an IP block called asynchronous FIFO which will take care of metastability filtering etc.
6. HLS is good at resource-dominated circuits (like DSPs) but not good at non-resource dominated circuits (like microprocessors)
7. If there is hold time violation only at one core, we can disable that core and still sell that chip as lower model, like i3, etc.
8. Include the discrete maths formula, liek $$\bar ab+ac=a+bc$$, etc.
