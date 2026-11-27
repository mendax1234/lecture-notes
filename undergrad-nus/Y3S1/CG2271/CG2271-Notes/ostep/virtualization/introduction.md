# Introduction

In this section, we will spend quite a lot of time talking about [virtualization](../introduction-to-os.md#virtualization), which we already have had a glimpse of. The main idea behind virtualization is to **share the limited resources**.

Assume there is one physical CPU in a system (though now there are often two or four or more). What **virtualization** does is

> take that single CPU and make it look like many virtual CPUs to the applications running on the system.

Thus, while each application thinks it has its own CPU to use, there is really only one. And thus the OS has created a beautiful illusion:

> it has **virtualized** the CPU.

In this part of the notes, we will introduce two important virtualizations:

1. CPU Virtualization: The process
2. Memory Virtualizatino: The address space

These two abstractions allow a program to run as if it is in its own private, isolated world; as if it has its own processor (or processors); as if it has its own memory.
