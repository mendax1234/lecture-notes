# Tut 04 - Introduction to POSIX Threads

## Problems

{% hint style="success" %}
This tutorial is structured as a set of experiments to get you familiar with POSIX threads. Thus, there is no coding or writing requirement.
{% endhint %}

### 2. Find the maximum of a large array

1. Each thread has its own **registers and user stack**. In C, each function call creates a **stack frame** on the thread's stack, and a function cannot directly access local variables stored in another stack frame unless they are explicitly passed to it or otherwise made accessible.
   1. For example, if `main()` creates a thread but does not pass its local variable `a` to that thread, the new thread **cannot directly access `a`**, because `a` resides in the stack belonging to the `main()` thread.

### 3. Race Conditions

1. For **Midterm and Tutorial 5**, we should know how to **systematically determine the minimum and maximum possible values of a shared variable** when two threads execute concurrently, by considering the possible interleavings of their operations.

### 4. Mutex

1. Operations that modify the state or value of a **semaphore or mutex are performed atomically**. The critical section doesn't need to be executed atomically!
