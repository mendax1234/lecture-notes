# Implicit FSM Traversal Methods

## State Extraction

> This part is **not covered in detail** in NUS EE4218.

The basic idea of state extraction is rather easy

1. Find all states reachable from an **initial state**, and add these states to the subset of reachable states.
2. From this subset, determine all the reachable states, and add them to the subset.
3. Repeat the above step, and stop when [convergence is reached](#user-content-fn-1)[^1].

For example, if our initial state is `001` and we have 2 inputs, meaning that we have 4 combinations of input to apply on any state including the initial state `001`.

* Let's say after trying all 4 input combinations on the initial state `001`, we find out that the next state can be `010` or `100`. Thus, we add these two states to our set of reachable states from `001`. The set is now, `{001, 010, 100}`.
* Now, we apply all 4 input combinations on `010` and `100` **respectively** and see what are the states that we can achieve.
* We repeat this until we have traversed all the states in the set of reachable states.&#x20;

[^1]: No new reachable states added to the subset.
