# Tut 06 - Semaphores

{% hint style="danger" %}
This is perhaps the most challenging tutorial so far! Thus need to pay close attention in the tutorial questions here! Every question is worth re-doing before the final exam!
{% endhint %}

## Problems

### 1. Easy Semaphore

This question is similar to how we analyze the possible outcomes of a data race condition. The normal approach to this kind of question it to do **brute-force**, however, the semaphores may help us eliminate some paths. For example, in this question, as process A `wait(S2)` with `S2=0` initially and S2 is only signalled in process C. This means that process A can only be executed after process C. So the possible paths in this question are:

1. C->A->B, which gives us 36 as final outcome.
2. C->B->A, which gives us 18 as final outcome.
3. B->C->A, which gives us 6 as final outcome.

{% hint style="warning" %}
Notice that even though in process C, `S2` is signaled earlier than `S1`, there's still a posibility that process B (which uses `S1`) will be executed before process A (which uses `S2` first).
{% endhint %}

### 2. Medium Semaphore

In this quesiton, we should be trained to know how to code semaphores in a **modular way**. By modular, I mean we should really try to decompose the problems into different parts know what parts need what kind of semaphores. For example, in this question:

1. We need to maintain a counter variable which counts the number of processes being blocked, and this should be protected by a mutex as multiple processes may or modify this variable.
2. We need to block the N-1 processes.
3. We need to wake up the blocked N-1 processes when the N-th process comes.

<details>

<summary>When is the process actually blocked?</summary>

In our lecture on [semaphores](https://app.gitbook.com/s/5cfVJRHhgDkjMTtQIPfW/concurrency/semaphores#semaphores), we know that in this module, we implement the `wait()` as follows:

{% code lineNumbers="true" expandable="true" %}
```
	wait(S):
    if S <= 0:
        block
    S = S - 1
```
{% endcode %}

Thus, the process calling `wait(S)` will be blocked before it is able to decrement the semaphore value.

</details>

### 3. Medium-High Semaphore

In question (c), the variable `pass` is simply a **Boolean flag indicating whether the villager has successfully entered the bridge**. For example, process A (villager 1) enters the bridge in direction 1, its `pass` will be true, meaning that he is now on the bridge. Then, let's say another process B (villager 2) wants to enter the bridge in an opposite direction, its `pass` will be false, and it will simply retry in the loop until nobody is crossing in direction 1.

### 4. High Semaphore

When a process is blocked by calling `wait(Semaphore)`, it must wait for **another process/thread** to call `signal(Semaphore)`. This may wake the blocked process, allowing it to continue executing its `wait()` operation and decrement the semaphore before continuing execution.
