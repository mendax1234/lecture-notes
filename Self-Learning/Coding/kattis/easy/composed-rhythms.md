# Composed Rhythms

## Question

{% embed url="https://open.kattis.com/problems/composedrhythms" %}

## Solution

### Idea

This is a **math** problem. Since the question is asking for one of the solutions, it is not that hard. My method is to find as many 3s as possible, and after minusing all these 3s, we are left with the following three cases:

1. the remainder is 2, that means we can use one 2 to satisfy the requirement
2. the remainder is 1, that means we need to spare one 3 out and use two 2s to satisfy the requirement
3. the remainder is 0, that means our division with all 3s are successful!

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/composedrhythms/composedrhythms.c" %}
