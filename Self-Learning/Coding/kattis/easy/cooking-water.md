# Cooking Water

## Question

{% embed url="https://open.kattis.com/problems/cookingwater" %}

## Solution

### Idea

This is again a **math** problem. The tricky point for me is in understanding the problem itself. But after translating it into easier problem, the problems becomes: _whether it is possible to have  a time `T` that is in range of all the input ranges._ So, more specifically speaking, this is a **window size** problem.

After translation, the problem becomes eaiser to solve, we just need to update the range from the start of our input and at last, see if the left bound is bigger than the right bound or not.

* If it is, that means, it is impossible to have such `T`.
* Otherwise, it is possible to have such `T`.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/cookingwater/cookingwater.c" %}
