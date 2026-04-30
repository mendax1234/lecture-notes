# Week 4

## Wednesday

### Problem

{% embed url="https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/description/?envType=study-plan-v2&envId=leetcode-75" %}

### Solution

I won't cover the trivial solution here. Instead, I will demo the two pointers strategy. This is pretty eye-opening!

So, in short, we have two ptrs, one `fast` and one `slow`. The `fast` moves twice the speed of `slow`. So, that once `fast` reaches the end of the linked list, the `slow` will be at the middle of the linked list. (Bare me with the minus/add 1 issue)

{% code lineNumbers="true" %}
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteMiddle(ListNode head) {
        if (head.next == null) {
            return null;
        }
        ListNode fast = head;
        ListNode slow = head;
        while (true) {
            fast = fast.next;
            if (fast.next == null) {
                break;
            }
            fast = fast.next;
            if (fast.next == null) {
                break;
            }
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```
{% endcode %}

## Thursday

### Problem

{% embed url="https://leetcode.com/problems/removing-stars-from-a-string/description/?envType=study-plan-v2&envId=leetcode-75" %}

### Solution

Again, the trivial solution, which is to find the first `*` and then delete its left character, won't be covered because it takes $$O(n^2)$$ and cannot pass all the test cases. Here, will introduce two more interesting solutions.

{% stepper %}
{% step %}
**Bracket matching problem**

We build a "stack" using the `StringBuilder`. Then when we iterate through the expression,

* if we encounter `*` (means closed bracket), we "pop" the content of the stack (our `StringBuilder`).
* else, we "push" the char from the expression (open bracket) into the stack

Lastly, we just convert our stack (`StringBuilder`) to a String.

{% code lineNumbers="true" %}
```java
class Solution {
    public String removeStars(String s) {
        // Bracket matching problem thinking
        char[] ca = s.toCharArray();
        StringBuilder res = new StringBuilder();
        for (char c : ca) {
            if (c == '*') res.deleteCharAt(res.length() - 1);
            else res.append(c);
        }
        return res.toString();
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
**Pointer moving Thinking**

Using the similar idea in solution 1 above, but a little bit different implementation. We can define a `ptr` to record the position we are writing char from our expression to. So, if we encounter `*`, we just move our `ptr` back by 1 and write nothing, and during the next writing, we may overwrite its left character with the new character. This is genius!

{% code lineNumbers="true" %}
```java
class Solution {
    public String removeStars(String s) {
        // ptr moving thinking
        char[] ca = s.toCharArray();
        int ptr = 0;
        for (char c : ca) {
            if (c == '*') ptr--;
            else ca[ptr++] = c;
        }
        StringBuilder res = new StringBuilder();
        for (int i = 0; i < ptr; i++) {
            res.append(ca[i]);
        }
        return res.toString();
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
