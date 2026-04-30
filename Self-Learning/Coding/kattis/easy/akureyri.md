# Akureyri\*

## Question

{% embed url="https://open.kattis.com/problems/akureyri" %}

## Solution

{% hint style="warning" %}
**Future Improvement:** Use Hash Table Idea to improve this in the future!
{% endhint %}

### Idea

The general idea for this idea is relative easy. Basically, you just need to count how many times each unique location has appeared.

However, to implement it in C, it is a bit troublesome. I am not sure whether there will be a easier way to do that, but in my `main` function, attention should be paid on how to deal with memory allocation.

{% code lineNumbers="true" %}
```c
int main()
{
  size_t n = cs1010_read_size_t();
  char **loc_arr = calloc(n, sizeof(char *));
  long *freq = calloc(n, sizeof(long));
  if (loc_arr == NULL)
  {
    return 1;
  }
  long len = 0;
  // Update freq and loc_arr
  for (size_t i = 0; i < n; i += 1)
  {
    char *name = cs1010_read_word();
    char *loc = cs1010_read_word();
    long duplicate = find_duplicate(loc_arr, (long)n, loc);
    if (duplicate != -1)
    {
      freq[duplicate] += 1;
      free(loc);
    }
    else
    {
      loc_arr[len] = loc;
      freq[len] += 1;
      len += 1;
    }
    free(name);
  }

  // Print result
  print_res(loc_arr, freq, (long)n);

  // Free Memory
  for (long i = 0; i < n; i += 1)
  {
    free(loc_arr[i]);
  }
  free(loc_arr);
  free(freq);
}

```
{% endcode %}

Here the general idea is as follows:

1. If `loc` is not duplicate, then we only need to free `name` .
2. Otherwise, we need to free `name` and `loc` .

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/akureyri/akureyri.c" %}
