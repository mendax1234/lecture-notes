# Babylonian Numbers

## Question

{% embed url="https://open.kattis.com/problems/babylonian" %}

## Solution

### Idea

**Time Complexity:** $$O(n)$$

This question is **not** a give-away question, it needs some ability to break the problem into smaller pieces to solve.

1. Iterate from the right to the left
2. Once we encounter ',' add the corresponding number in base-60 to our sum
3. Otherwise, update the corresponding number.

The implementaion logic can be seen from the code as follows:

{% code lineNumbers="true" %}
```c
for (size_t i = 0; i < n; i += 1)
{
  size_t j = strlen(input[i]) - 1;
  size_t sum = 0;
  size_t sum_pow = 0;
  size_t cur = 0;
  size_t cur_pow = 1;
  while (input[i][j] != 0)
  {
    if (input[i][j] == TERMIN)
    {
      sum += cur * power(60, sum_pow);
      cur = 0;
      cur_pow = 1;
      sum_pow += 1;
    }
    else
    {
      cur += ((size_t)input[i][j] - (size_t)'0') * cur_pow;
      cur_pow *= 10;
    }
    j -= 1;
  }
  sum += cur * power(60, sum_pow);
  result[i] = sum;
}
```
{% endcode %}

Line 24 is to process the "first" number.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/babylonian/babylonian.c" %}
