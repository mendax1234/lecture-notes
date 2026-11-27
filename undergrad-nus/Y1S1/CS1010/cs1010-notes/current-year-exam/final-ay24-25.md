# Final (AY24/25)

## Problems

### 11. Call Stack Diagram

The first tricky point is in the declaration of the function `inn`. Here `long *p[2]` seems to create an array, however, it is actually declaring a pointer to pointer here! And when drawing the call stack diagram, you should only draw one variable, which is a pointer to pointer that points to the `p` in the `main`. (See more at [#pass-an-array-to-a-function](../lec-tut-lab-exes/lecture/lec-06-call-stacks-arrays/#pass-an-array-to-a-function "mention"))

The tricky point in this question actually lies in "printing" part. Is it a _crash_ or _undefined behavoir_? Not too sure for now.

But an interesting point here is that the actual output of this program is "30". That's crazy right! This let's me have a deeper thinking about what's happening here. Now, the question is, how is the stack frame of `inn` not cleared/destroyed?

This reminds me of the working principle of calling a program in C. Indeed, we are using the idea of "stack". But the actual working principle of "removing" a stack is **not clearing all its content** but moving an amazing stuff called "stack pointer" back to its "jumping" location (sorry for this kind of weird words cuz I forgot the jargons :joy:). (If you want further discussion, I think I need to study/review more about the Computer Organization part, iirc there is also something called `pc` (program counter), this will help us decide the exact location when our stack pointer moves back).

So, in this question, after calling `inn`, we move over stack pointer back to memory location that stores the instruction `inn(p)` (this will have the effect of "destroying" the `inn` stack frame, but actually it is not destroyed!) and maybe our `pc` will contain the address of the next instruction to be executed, which is `cs1010_println_long(*p[1])`. And then, we will execute the "print" instruction here. But notice or not, the content in the memory location of `inn` stack frame is still there! So theoritically speaking, we can still access that location and won't generate any error (from the computer organization side).

> Okie, I just found that I am probably wrong, when calling `inn`, we are branching to another location maybe. :joy: Nvm, wait for future me to correct this bah hahahaha

And another reason why we are lucky (still get 30) is because we are not calling other functions, which will overwrite the content in the `inn` stack frame).

### 12. Recursion

> This year the recursion seems to be a bit "easier" hahaha.

The most common mistake I can think of (myself :joy:) is to try the largest coin every time. This is not the optimal solution. e.g. If your input is 40, using the "largest" method, you will get 3, which is $$40=20+15+5$$. But the optimal solution should be 2, which is $$40 = 20+20$$.

But, this wrong method can somehow give us some hints on how to arrive on the optimal solution. That's for every **possible** coin, we find the number of candidates it needs and update `min` to be the option with the smallest number of candidates.

{% code lineNumbers="true" %}
```c
long num_of_coins(long amount)
{
  if (amount == 0)
  {
    return 0;
  }
  coins[5] = {5, 10, 20, 25, 50};
  long min = LONG_MAX;
  for (i = 0; i < 5; i += 1)
  {
    if (amount >= coins[i])
    {
      long candidate = num_of_coins(amount - coins[i]) + 1;
      if (candidate < min)
      {
        min = candidate;
      }
    }
  }
  return min;
}
    

```
{% endcode %}
