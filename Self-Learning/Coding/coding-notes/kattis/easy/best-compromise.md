# Best Compromise

## Question

{% embed url="https://open.kattis.com/problems/compromise" %}

## Solution

{% hint style="warning" %}
**Future Improvement:** Use the advanced property (e.g. vector) of Java/C++ to solve this question.
{% endhint %}

### Idea

This question is an okie question to test your ability of "Decomposition". The only thing you should notice is that the final _agreement_ should be "generated" instead of from one of the existing input!

To "generate" the agreement, the fastest way I can think of is to get the average of each digit, and

1. if the average < 0.5, the digit should be 0.
2. Otherwise, it should be 1.

Here is the code snippet for this "average" part:

```c
/*
 * @param str_arr       the input word array
 * @param n             the number of lines
 * @param m             the length of each line
 *
 * @output a pointer that points to a memory location storing the agreement
 */
char* get_agree(char **str_arr, size_t n, size_t m)
{
  long min_diff = LONG_MAX;
  char *min_str = calloc(m + 1, sizeof(char));
  for (size_t i = 0; i < m; i += 1)
  {
    long total = 0;
    for (size_t j = 0; j < n; j += 1)
    {
      total += str_arr[j][i] - '0';
    }
    double avg = (double)total / (double)n;
    if (avg < 0.5)
    {
      min_str[i] = '0';
    }
    else
    {
      min_str[i] = '1';
    }
  }
  min_str[m] = 0;
  return min_str;
}
```

### Code

#### C

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/compromise/compromise.c" %}
