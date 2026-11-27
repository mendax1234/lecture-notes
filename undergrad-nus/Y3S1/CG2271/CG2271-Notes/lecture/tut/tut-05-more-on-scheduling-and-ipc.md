# Tut 05 - More on Scheduling and IPC

## Problems

### 1. Process Scheduling

One major drawback of the **Round Robin scheduling algorithm** is that:

1. **Turnaround time may increase** because tasks are frequently interrupted and resumed, especially when the time quantum is small.
2. **Context switching introduces overhead**, since switching between tasks is **really slow**!

{% hint style="success" %}
Context switch involves switching the relevant **OS, memory, and hardware contexts** so that the CPU can resume execution of the new process correctly.
{% endhint %}

### 2. Share Memory IPC

1. `shmat()` **attaches a shared-memory segment to** a pointer in user space so that we can use the pointer to access the shared memory. The shared-memory segment itself is managed by the kernel and thus living in the kernel space.
2. After each `fork()`, the parent and child process have **separate copies of their own variables**. Therefore, changes to ordinary variables in one process do not affect the other, which allows each process to maintain its own independent state.
