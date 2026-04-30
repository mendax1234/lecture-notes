# Best Relay Team\*

## Question

{% embed url="https://open.kattis.com/problems/bestrelayteam" %}

## Solution

{% hint style="warning" %}
**Future Improvement**: Use the vector from C++ or the equivalent idea from Java to solve this problem again.
{% endhint %}

### Idea

This is a pretty interesting problem and utilises **Decomposition** gracefully! Lemme me dig it into smaller parts.

Firstly, to make this problem easier, let's define a `struct`called `athelete`to store the `name`, `first`(first sprinting time) and `second` (second sprinting time). And then to get all the input, we just need to use a `struct` array.

Then, the idea is as follows:

1. Sort the `struct` array based on the second sprinting time.
2. Iterate through the sorted array **once** to try all the possible first sprinting time and find the remaining minimum second sprinting time. Compare it with the minimum total to find the final minimum total time and the final team.

### Code

#### C

The implementation in C is quite troublesome because:

1. You need to deal with the memory allocation and deallcation manually
2. You need to implement a sorting algorithm

Among them, the first point is legit tricky. For me, my implementation is to **call** `calloc()` **once** only when reading the runners name. And then to update `cur_team` and `final_team`, I only do the assignment of the pointers instead of allocating the memory again.

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/bestrelayteam/bestrelayteam.c" %}
