# Lec 05 - Priority Queue

## Definition

In a priority queue, each element has a "priority" and an element with higher priority is served before an element with lower priority.

{% hint style="info" %}
In the later implementation using binary max heap, we may notice that this "priority" is related to the magnitude of the number, but not exactly everywhere, but **on the same level**.
{% endhint %}

A Binary (Max) Heap is a [complete binary tree](https://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees) that maintains the [Max Heap property](https://en.wikipedia.org/wiki/Binary_heap).

* **Complete Binary Tree**: Every level in the binary tree, except possibly the last/lowest level, is completely filled, and all vertices in the last level are as far left as possible
* **Binary Max Heap property**: The value of a vertex — except the leaf/leaves — must be greater than (or equal to) the value of its one (or two) child(ren).

Below is an example of a binary max heap from **visualgo**. And remember this general shape of a complete binary tree as it will be useful when we analyze the basic operation inside ths binary max heap.

<figure><img src="../.gitbook/assets/lec05-binary-max-heap-example.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
#### Additional Information

1. **Height**: the height of a tree is the **number** of edges from the top-most node to the farthest node in a subtree. For example, in the figure above, the height of this complete binary tree is 3.
{% endhint %}

## Basic Manipulation

A complete binary tree can be stored efficiently as a compact array A as there is no gap between vertices of a complete binary tree/elements of a compact array. (Go back to [visualgo](https://visualgo.net/en/heap?slide=2-2) to visualize this process).

This way, we can implement basic binary tree traversal operations with simple index manipulations (with help of [bit shift manipulation](https://visualgo.net/en/bitmask)):

1. `parent(i) = i>>1`, index `i` divided by 2 (integer division),
2. `left(i) = i<<1`, index `i` multiplied by 2,
3. `right(i) = (i<<1)+1`, index `i` multiplied by 2 and added by 1.

### Insert

Insertion of a new item `v` into a Binary Max Heap can only be done at the _last index N plus 1_ to maintain the compact array. Why?

> This is to maintain the **complete binary tree** property.

However, the **Max Heap property** _may_ still be violated. This operation then fixes Max Heap property from the insertion point upwards (if necessary) and stop when there is no more Max Heap property violation. (See the visualization on [visualgo](https://visualgo.net/en/heap?slide=4) to understand it better)

### ExtractMax

The method returns and deletes the **root** vertex, then replace the root with the _last index_ **N**. This is also to maintain the **complete binary tree** property.

But after the replacement, it will very likely violates the **Max Heap property**. This operation then fixes Binary Max Heap property from the root downwards by comparing the current value with the its child/the larger of its two children (if necessary). (Again, see the visualization on [visualgo](https://visualgo.net/en/heap?slide=5) to understand it better)

### Create

To create a binary max heap, one trivial way is to call the [#insert](lec-05-priority-queue.md#insert "mention") N times. Thus, its time complexity will be $$O(n\log n)$$. However, to make it faster, there is the second way to create a binary max heap,

1. insert each vertex sequentially, don't care about the max heap property
2. ignore all the bottom leaves, and start fixing the max heap property from the second-last vertex, one-by-one

More analysis of the second creation will be done in lec 05b.

### HeapSort

HeapSort is just simply calling the [#extractmax](lec-05-priority-queue.md#extractmax "mention") operation **N** times. Thus, its time complexity is obviously $$O(n\log n)$$. (See the visualization on [visualgo](https://visualgo.net/en/heap?slide=8)!)

One advantage of Heapsort is that we can use it to achieve **partial sort**! (Its real world application includes the searching result you get from Google). And below is an application from LeetCode 😂

{% embed url="https://leetcode.com/problems/kth-largest-element-in-an-array/description/?envType=study-plan-v2&envId=leetcode-75" %}

For the explanation, please see from here!
