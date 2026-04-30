# Final (AY23/24)

Problems

Question paper without answers:

{% file src="../../.gitbook/assets/final-2324-s1.pdf" %}
Question without Answers
{% endfile %}

Question paper with answers:

{% file src="../../.gitbook/assets/final-2324-s1-with-comments.pdf" %}
Question with Answers
{% endfile %}

### 11. Sorting

**Insertion sort** is fast when it comes to sorting an _almost sorted_ list.

Here is the table summarise the best-case, worst-case time complexity for different sorting algorithms:

| Sorting algorithm | Sorted array (best case) | Inversely sorted array (worst case) | General array (average case) |
| ----------------- | ------------------------ | ----------------------------------- | ---------------------------- |
| Bubble Sort       | 0                        | $$O(n^2)$$                          | $$O(n^2)$$                   |
| Insertion Sort    | 0                        | $$O(n^2)$$                          | $$O(n^2)$$                   |
| Selection Sort    | $$O(n)$$                 | $$O(n)$$                            | $$O(n)$$                     |

### 12. Sorting

Notice that in this question, we are talking about the number of _swaps,_ not the running time. So, for the **selection sort** we are always making $$n$$ times of _swap._

Below is the table summarises the number of _swap_ needed for different sorting algorithms:

| Sorting algorithm | Sorted array (best case) | Inversely array (worst case) | General array (average case) |
| ----------------- | ------------------------ | ---------------------------- | ---------------------------- |
| Bubble Sort       | 0                        | $$O(n^2)$$                   | $$O(n^2)$$                   |
| Insertion Sort    | 0                        | $$O(n^2)$$                   | $$O(n^2)$$                   |
| Selection Sort    | $$O(n)$$                 | $$O(n)$$                     | $$O(n)$$                     |

### 16. Recursion

> This kind of sequence (each binary number is one-bit different from its previous) is called _grey code_! Amazing right!

In this problem, it utilises a _hidden information_: that is the string is intialized to be all 0! So, it is safe for the first line of our recursion to be just `generate(str, n, k + 1)` since once it reaches the end, it will print all `0`.

Then our idea may be a bit clearer, we modify our current bit `str[k]` to be different from what we print out previously, and then print it out by calling `generate(str, n, k + 1)` again.

{% code lineNumbers="true" %}
```c
generate(str, n, k + 1);
str[k] = (str[k] == '0') ? '1' : '0';
generate(str, n, k + 1);
```
{% endcode %}

To make our mind clearer, Line 1 is to generate the "previous" binary string, then Line 2 modifies the "next" binary string to be exactly one bit different from the previous and then print it out calling the substring recursively.
