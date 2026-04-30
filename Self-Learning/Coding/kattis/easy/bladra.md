# Blaðra

## Question

{% embed url="https://open.kattis.com/problems/bladra" %}

## Solution

### Idea

The idea for this problem is rather easy and it utilises **linear search**. We just need to build a frequency table to store the times each problem has been solved. And then find the minimum number of times.

{% hint style="info" %}
Note that a small point to accelerate is that if you find a 0 in your frequency table, you can safely state that the minimum is 0 and don't need to search the remaining.
{% endhint %}

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/bladra/bladra.c" %}
