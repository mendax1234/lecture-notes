# Apaxiaaaaaaaaaaaans!

## Question

{% embed url="https://open.kattis.com/problems/apaxiaaans" %}

## Solution

### Idea

This question I would say is another pretty elegant question. The genius idea here is to store the char you have printed `temp` and compare the char from your source character with `temp`, if it is the same, move on. Otherwise, update print the different char and update `temp`.

{% code lineNumbers="true" %}
```c
int main()
{
  char *word = cs1010_read_word();
  long i = 1;
  putchar(word[0]);
  char temp = word[0];
  while (word[i] != 0)
  {
    if (word[i] != temp)
    {
      putchar(word[i]);
      temp = word[i];
    }
    i += 1;
  }
  free(word);
}

```
{% endcode %}

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/apaxiaaans/apaxiaaans.c" %}
