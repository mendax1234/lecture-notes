# Lab 02 - Introduction to Hardware/Software Co-Design

In Lab 02, we are going to configure our FPGA and write a C program to enable the feature of sending data and receiving data to and from the coprocessor via UART. The sending and receiving process is shown in the following diagram.

<figure><img src="../.gitbook/assets/lab02-intro.svg" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
The orange line in the coprocessor module indicates that in this lab, we will just do the "loopback", which means that the coprocessor won't do any calculation and this matrix multiplication job is done **on the ARM A53 processor** on the PS instead. In the future lab, we will open this loop.
{% endhint %}
