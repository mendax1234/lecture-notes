# Anti-Palindrome

## Question

{% embed url="https://open.kattis.com/problems/antipalindrome" %}

## Solution

### Idea

This is a pretty awesome question I think. The first thing is to break this problem into smaller problems. We may find that this problem can be solved by writing two functions below:

1. `palindrome(cahr *str, long start, long end)` that judge whether a string from `start` to `end` is a palindrome or not.
2. Iterate through **all** the substrings of at least length 2 from the given string.

#### palindrome

The implementation can be seen in [#code](anti-palindrome.md#code "mention"). It is a basic recursive function that utilises wishful thinking. Here I have used some functions from `<ctype.h>` to simplify my life.

{% code lineNumbers="true" %}
```c
bool panlidrome(char *str, long start, long end)
{
  if (start >= end)
  {
    return true;
  }
  if (!isalpha(str[start]))
  {
    return panlidrome(str, start + 1, end);
  }
  if (!isalpha(str[end]))
  {
    return panlidrome(str, start, end - 1);
  }
  if (tolower(str[start]) != tolower(str[end]))
  {
    return false;
  }
  return panlidrome(str, start + 1, end - 1);
}

```
{% endcode %}

#### Iterate all the substrings of length 2

This is a key/awesome idea we can take-away from this problem. Here, I utilize two `for` loops as follows:

{% code lineNumbers="true" %}
```c
  for (long start = 0; start < len - 2; start += 1)
  {
    for (long end = start + 2; end < len; end += 1)
    {
      if (panlidrome(line, start, end))
      {
        cs1010_println_string("Palindrome");
      }
    }
  }

```
{% endcode %}

The awesome idea here is to use two loops, one stores `start`, the other stores `end`. The `end` is always at least 2 characters behind start and must be smaller than `len`. So, the start must be smaller than `len-2`.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/antipalindrome/antipalindrome.c" %}
