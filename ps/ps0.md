# PS0

## Problems

### C. Mörk

This problem may not be easy to understand at first glance. But, given two inputs/conditions below

1. The total goals that two teams have scored in this game
2. the number of team (0/1/2) that has scored in this game

There are only a certain of cases that Siggi could tell which team actually wins. Find the condtion for these certain cases, and this is the key to solve this problem.

### D. Stafsetning

This is not an easy question, but thanks someone's pointing out at the course's discord channel, the procedure to solve this problem is

Here’s the rewritten and reordered procedure in a clean, logical flow:

1. **Calculate the total number of typos** from all problems.&#x20;
2. **Check feasibility**: if  $$m<k$$  (i.e., fixing one typo takes longer than the available time per day), then Unnar can’t finish. 🙁
3. **Calculate typos fixed per day**:  $$\text{typos\_per\_day} = \lfloor k / m \rfloor$$. (floor of  $$k/m$$)
4. **Calculate the least number of days needed**: $$\lceil \text{total\_typos} / \text{typos\_per\_day} \rceil$$. (ceil of total\_typos / typos\_per\_day)

{% hint style="warning" %}
#### `int` and `long` in Java

This is probably the biggest tricky point in this problem. If you define `total_typos` as `int`, you won't be able to pass all the test cases. This is because the `total_typos` may exceed the largest range of `int`, so to meet the problem's requirement, we should define `total_typos` as `long`.


{% endhint %}

#### `int` and `long` difference in Java

| Data Type | Bit Size | Signed Range (approx)                          | Unsigned Range (approx)     |
| --------- | -------- | ---------------------------------------------- | --------------------------- |
| `int`     | 32-bit   | $$-2.1 \times 10^9~to~+2.1 \times 10^9$$       | $$0~to~ 4.3 \times 10^9$$   |
| `long`    | 64-bit   | $$-9.2 \times 10^{18}~to~+9.2 \times 10^{18}$$ | $$0~to~1.8 \times 10^{19}$$ |

### E. Bilað Lyklaborð

Things to collect from the problem

1. **Normal practice to traverse through a string in Java**

### &#x20;**G.** Hakkari

Things to note down from this problem

1. Create and read into a multi-dimensional array in Java
2. When to consume the new line character `\n`

### H. Fullkomin mylla

Things to collect from this problem

1. When doing nested loop string traversal, how to keep the "pointer" to the current character

### I. Ég elska hann

This is probably the most interesting problem is PS0. It is a math problem wrapped by a skin of coding😂. And below is a very inspirational video (but try to solve it before watching this video)

{% embed url="https://www.youtube.com/watch?v=uCsD3ZGzMgE" %}

{% hint style="info" %}
#### Small Tips

Start with some obvious example, and try to find the pattern.
{% endhint %}

If you are curious, I happen to encounter this famous Josephus problem's idea in a very famous magic!

### J. Bergur

This is a problem I have encounter when doing open kattis, and the notes for it is as follows:

{% embed url="https://wenbo-notes.gitbook.io/coding/kattis/easy/bergur" %}
