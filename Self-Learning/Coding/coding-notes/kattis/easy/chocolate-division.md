# Chocolate Division\*

## Question

{% embed url="https://open.kattis.com/problems/chocolatedivision" %}

{% hint style="info" %}
There is an ungiven condition in this question, that is Alf will play first.
{% endhint %}

## Solution

### Idea

This is a pretty awesome question to test your **math** thinking a.k.a **pattern recognition**! The easiest way to solve this question is to use the idea of `total_cut`, which means how many available cuts for a bar of chocolate have given that it is $$r\times c$$. The answer is $$r\times c - 1$$

Known that, since Alf plays first, we can easily find the pattern, which makes sense in our mind also

1. If `total_cut` is odd, that means Alf will win. (It is easy to understand)
2. Otherwise, Beata will win.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/chocolatedivision/chocolatedivision.c" %}
