# A Second Opinion

## Question

{% embed url="https://open.kattis.com/problems/secondopinion" %}

## Solution

### Idea

To break this problem into several small problems:

1. Find the **hours**
2. Find the **minutes**
3. Find the **seconds**

For hours, we just need to use integer division `/`, `hours = input secs / 3600`. After we get the hours, substract it from our `input secs`.

Do the similar operation on `minutes`. The remaining are `seconds`.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/secondopinion/secondopinion.c" %}
