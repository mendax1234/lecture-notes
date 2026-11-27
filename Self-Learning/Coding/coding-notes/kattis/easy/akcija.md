# Akcija

## Question

{% embed url="https://open.kattis.com/problems/akcija" %}

## Solution

### Idea

Seems that it is a sorting problem.

1. Sort the price in decreasing order. (The problem seems that we need a fast sorting algorithm, the normal $$O(N^2)$$ method will cause TLE)
2. Iterate through each price element, add the first two in the group only.

The second step can be done as follows elegantly:

{% code lineNumbers="true" %}
```c
long calc_min_price(size_t n, long a[n])
{
  size_t cur = 0;
  long price = 0;
  for (size_t i = 0; i < n; i += 1)
  {
    if (i % 3 != 2)
    {
      price += a[i];
    }
  }
  return price;
}
```
{% endcode %}

### Code

