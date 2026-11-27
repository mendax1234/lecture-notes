# Final (AY21/22)

## Problems

Question paper without answers:

{% file src="../../.gitbook/assets/final-2122-s1.pdf" %}
Question without Answers
{% endfile %}

Question paper with answers:

{% file src="../../.gitbook/assets/final-2122-s1-with-comments.pdf" %}
Question with Answers
{% endfile %}

### 3. Variable and Proof

1. For `long` variables, it can be **negative!** Always try to think about negative number when trying to form the counter example.
2. The keyword "if and only if" needs to prove **both forwardly and reversely**!

### 8. Memory Allocation

The code below is a classic memory allocation error:

{% code lineNumbers="true" %}
```c
long *a = malloc(n * sizeof(long *));
```
{% endcode %}

Here `a` should be a pointer pointing to an array of `long`, but here you assume it should be pointing to to an array of `long *`. The implementation above is wrong. The correct one should be

{% code lineNumbers="true" %}
```c
long **a = malloc(n * sizeof(long *));
```
{% endcode %}

### 11. Time Complexity

The two recurrence relation below has the same Time Complexity:

$$
\begin{align*}
T(n)&=nT(n/2)+1 \\
T(n)&=nT(n/2)+n
\end{align*}
$$

And the time complexity is $$O(n^{logn})$$

### 17. Loop Invariant

> A good question to try practicing!

To get the **strong** and **correct** loop invariant in this question is a bit tricky. In comments, you can clearly see if you use the weak but correct loop invariant `a[l] < a[r+1 .. len-1] || a[r] <= a[0 .. l-1]`. It will be impossible to do the next question.

So, I would say the tip I get to do this kind of question is that:

1. Try to find a counter example to prove the **given loop invariant** is **incorrect**
2. To find the correct one, think more about **what the code is doing** rather than **giving out the exact expression directly** because sometimes the loop invariant even cannot be expressed in the exact expression. e.g. in this problem, the **strong** and **correct** loop invariant is "the minimum element of the array is in `a[l .. r]`"
