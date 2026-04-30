# Booking a Room

## Question

{% embed url="https://open.kattis.com/problems/bookingaroom" %}

## Solution

### Idea

**Time Complexity**: $$O(n)$$

My idea is to use **frequency table**, which is part of [**counting sort**](https://app.gitbook.com/s/KipySCGxC8NC1UpA24DS/lec-tut-lab-exes/lecture/lec-09-searching-and-sorting#counting-sort). The first $$O(n)$$ is used to get the input and it cannot be optimized further I think. The second $$O(n)$$ is to use **linear search** to find the available room, which I think cannot be modified either because there is no pattern, so **binary search** doesn't work.

### Code

{% @github-files/github-code-block url="https://github.com/mendax1234/Coding-Problems/blob/main/kattis/bookingaroom/bookingaroom.c" %}
