# Backspace

## Question

{% embed url="https://open.kattis.com/problems/backspace" %}

## Solution

### Idea

**Time Complexity:** $$O(n)$$

This is a pretty good question regarding **string manipulation**. The whole idea is shown below:

{% code lineNumbers="true" %}
```c
int main()
{
  char *line = cs1010_read_word();
  char *result = calloc(strlen(line), sizeof(char));

  long index = 0;
  for (long i = 0; line[i] != 0; i += 1)
  {
    if (line[i] == BACKSPACE && index > 0)
    {
      index -= 1;
    }
    else
    {
      result[index] = line[i];
      index += 1;
    }
  }
  result[index] = 0;
  printf("%s\n", result);
  free(line);
  free(result);
}

```
{% endcode %}

The soul/genius point in this question is regarding the use of `index`. This variable is used to store the location we will write to in our `result` string. And when we encounter `<`, we just move our `index` back by 1, so next time we can overwrite or move back again. This is awesome!

{% hint style="info" %}
This structure also gives us a new way to use `for` loop to iterate through a string! That is to set the terminating condition to be `line[i] != 0`.

Always keep in mind that the optimal way to loop through a string is to visit the index of the char array instead of changing the pointer!
{% endhint %}

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/backspace/backspace.c" %}
