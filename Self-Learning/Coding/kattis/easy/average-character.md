# Average Character

## Question

{% embed url="https://open.kattis.com/problems/averagecharacter" %}

## Solution

### Idea

The basic idea is easy, just to get the average ASCII value of each character in the input string.

However, if C is the language being used, things get a bit tricky regarding the input. Here, we can only use `fgets()` to read the input because white space characters cannot be ignored in this question. So, the correct way to read the input is below:

```c
#define MAX_LEN 102

if (fgets(string, MAX_LEN, stdin))
{
  string[strcspn(string, "\n")] = 0;
}
```

{% hint style="info" %}
Here the `MAX_LEN` is 2 characters more than 100 (the actual length of the string). This is because:

1. Add 1 to take the null character into account (the property of string in C)
2. Add 1 to take the `\n` character which is read by `fgets()` (the property of `fgets()` in C)
{% endhint %}

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/averagecharacter/averagecharacter.c" %}
