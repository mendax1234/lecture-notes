# Afjörmun

## Question

{% embed url="https://open.kattis.com/problems/afjormun" %}

## Solution

### Idea

I believe the tricky part for this question is to deal with the input in C. The main algorithm is not that hard. It is just you upper the first letter and iterate all the way till the end, if it `isupper()`, then turn it to lower letter. Otherwise, just leave it as it is.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/afjormun/afjormun.c" %}

## Take-away

### Read a line in C

To read a line in C, it is "safer" to use `fgets()`. See more about it in [fgets](https://app.gitbook.com/s/KipySCGxC8NC1UpA24DS/lec-tut-lab-exes/lecture/lec-11-strcut-and-standard-i-o#fgets "mention").

{% code lineNumbers="true" %}
```c
char line[MAX_LEN];

if (fgets(line, MAX_LEN, stdin))
{
  line[strcspn(line, "\n")] = 0;
}
```
{% endcode %}

`MAX_LEN` should be the actual length of the string +1 because `fgets()` may read the `\n` character!

{% hint style="info" %}
Note that sometimes this method is not 100% safe. So, the safest way I recommend is to use cs1010 library.
{% endhint %}

### Read an integer with newline

Suppose we input **an** integer with `\n`, to read it into a variable using `scanf()`, use the code below:

{% code lineNumbers="true" %}
```c
int a;
scanf("%d", &a);
```
{% endcode %}

### Read several integers with newline

This will be a bit tricky. For the first integer, we can still use the method above [#read-an-integer-with-newline](afjormun.md#read-an-integer-with-newline "mention"). But for the remaining integers, we should use the following method to read:

{% code lineNumbers="true" %}
```c
scanf("\n%d", &a);
```
{% endcode %}

The leading `\n` is used to consume the `\n` that is left in the `stdin`.
