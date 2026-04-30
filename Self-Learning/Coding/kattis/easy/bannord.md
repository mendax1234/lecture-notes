# Bannorð

## Question

{% embed url="https://open.kattis.com/problems/bannord" %}

## Solution

### Idea

This is another pretty awesome question about **string manipulation**! My idea is as follows:

1. Iterate through each forbidden letter in the line
2. replace the words with '\*'

To implement this idea, I have used `strchr()` in the `<string.h>` from the [library.md](../../baisc-knowledge/c/library.md "mention"). And one of the major functions `find_and_replace()` will do the work of finding the location of the first occurence of the forbidden letter. Use the property of `strchr()`, we will do the [Pointer Arithmetic](https://app.gitbook.com/s/KipySCGxC8NC1UpA24DS/lec-tut-lab-exes/lecture/lec-07-pointers-memory-management#pointer-arithmetic "mention") to get the position of this letter in our line. And call the `replace()` function to replace the entire word with `*`.

{% code lineNumbers="true" %}
```c
void find_and_replace(char forbid_letter, char *line)
{
  char *first_occur_ptr = strchr(line, forbid_letter);
  while(first_occur_ptr != NULL)
  {
    long pos = first_occur_ptr - line;
    replace(line, pos);
    first_occur_ptr = strchr(line, forbid_letter);
  }
}
```
{% endcode %}

To replace the word with `*`, given that we know the position of the forbidden letter, we need to replace the left remaining and right remaining letters in the word. The implementation logic is shown as follows:

{% code lineNumbers="true" %}
```c
void replace(char *line, long pos)
{
  // Left replacement
  for (long i = 0; pos-i >= 0 && line[pos-i] != WHITESPACE; i += 1)
  {
    line[pos-i] = REPLACE;
  }
  // Right replacement
  for (long i = 1; line[pos+i] != '\n' && line[pos+i] != WHITESPACE; i += 1)
  {
    line[pos+i] = REPLACE;
  }
}   
```
{% endcode %}

{% hint style="info" %}
Notice that whenever do the string manipulation, use an `index` integer and pay attention to the **edge cases**. Usually, these edge cases should be put at the first part of the `&&` operator for [Short-Circuiting](https://app.gitbook.com/s/KipySCGxC8NC1UpA24DS/lec-tut-lab-exes/lecture/lec-04-conditionals#short-circuiting "mention") reasons.
{% endhint %}

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/bannord/bannord.c" %}
